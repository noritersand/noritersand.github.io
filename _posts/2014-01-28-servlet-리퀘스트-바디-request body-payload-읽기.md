---
layout: post
date: 2014-01-28 14:05:00 +0900
title: '[Servlet] 리퀘스트 바디(request body, payload) 읽기'
categories:
  - servlet
tags:
  - java
  - serlvet
  - request-body
  - payload-body
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-14#section-3.2](https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-14#section-3.2)
- [http://stackoverflow.com/questions/23118249/whats-the-difference-between-request-payload-vs-form-data-as-seen-in-chrome](http://stackoverflow.com/questions/23118249/whats-the-difference-between-request-payload-vs-form-data-as-seen-in-chrome)
- [http://kukuta.tistory.com/95](http://kukuta.tistory.com/95)


## 개요

request body, request body post data, payload, payload body 등으로 불리는 리퀘스트 바디는 바디를 통해 전송한 어떤 값이 쿼리스트링이나 `x-www-form-urlencoded` 타입과 다르게 이름이 없어서 특수한 방식으로 해석해야 하는 형태인 것을 말한다.

일반적으로 POST, PUT, PATCH 등의 메서드에서 사용한다. GET, DELETE 메서드에서도 쓸 수 있지만 권장되지 않는다. 조회(혹은 조회해서 삭제)를 처리하는 메서드에서는 굳이 써야할 필요가 없고, 서버에서 리퀘스트 바디를 처리할 때 더 많은 리소스를 사용하기 때문이다.

전송되는 값은 JSON이나 XML과 같은 다양한 형식일 수 있는데 이를 Content-Type 헤더로 서버측에 알려줘야 한다.

아래는 리퀘스트 바디를 사용하는 HTTP 요청의 원문이다:

```bash
POST /take/my/awesome/draft HTTP/1.1
Content-Type: application/json
Accept: */*
Host: localhost:8080
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Length: 37
Cookie: SESSIONID=MTctZOTjA5NZVLMMkxTkUMxMi0Gg2MDhAtwTyD0DkzmTOODZ
 
{
"num": 1,
"bool": true
}
```


## 리퀘스트 바디 구현 예시

### 클라이언트

```html
<script>
  $.ajax({
    type: 'post',
    url: '/ajaxtest.data?id=1',
    data: '[{"name1":"value1", "name2":"value2"}, {"name1":"value3", "name2":"value4"}]',
    dataType: 'json',
    contentType: 'application/json; charset=utf-8',
    success: function(data) {
      ob = data;
    },
    error: function(jqXHR, textStatus, errorThrown) {
      console.log(jqXHR);
      console.log(textStatus);
      console.log(errorThrown);
    }
  });
</script>
```

### 서버

```java
package com.test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class Dispatcher extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        process(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        process(req, resp);
    }

    protected void process(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        System.out.println(readBody(req));
    }

    public static String readBody(HttpServletRequest request) throws IOException {
        BufferedReader input = new BufferedReader(new InputStreamReader(request.getInputStream()));
        StringBuilder builder = new StringBuilder();
        String buffer;
        while ((buffer = input.readLine()) != null) {
            if (builder.length() > 0) {
                builder.append("\n");
            }
            builder.append(buffer);
        }
        return builder.toString();
    }
}
```

### 출력 값

```
[{"name1":"value1", "name2":"value2"}, {"name1":"value3", "name2":"value4"}]
```
