---
layout: post
date: 2014-01-28 14:05:00 +0900
title: 'Servlet: Request에서 payload body(request body) 읽기'
categories:
  - java
  - servlet
tags:
  - java
  - serlvet
  - request body
  - payload body
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-14#section-3.2](https://tools.ietf.org/html/draft-ietf-httpbis-p3-payload-14#section-3.2)
- [http://stackoverflow.com/questions/23118249/whats-the-difference-between-request-payload-vs-form-data-as-seen-in-chrome](http://stackoverflow.com/questions/23118249/whats-the-difference-between-request-payload-vs-form-data-as-seen-in-chrome)
- [http://kukuta.tistory.com/95](http://kukuta.tistory.com/95)

HTTP에는 어떠한 유형의 내용이 전송되는지를 의미하는 Content-Type 헤더가 있다. Content-Type은 미디어타입(구: MIME)이라고도 한다.

서블릿에선 이 헤더의 값이 application/json 혹은 multipart/form-data일 때, 일반적인 경우와 다르게 HTTP 메시지 바디 라인을 직접 읽는 방식을 사용해야만 전송 데이터에 접근할 수 있다.

이 때 사용되는 API는 `ServletRequest.getInputStream()`, `ServletRequest.getReader()`가 있으며, 이런 형태의 전송 데이터를 'payload body' 혹은 'request body post data'라고 한다.

## send

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

## receive

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

## console 출력

```
[{"name1":"value1", "name2":"value2"}, {"name1":"value3", "name2":"value4"}]
```
