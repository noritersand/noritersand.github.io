---
layout: post
date: 2014-01-14 14:03:00 +0900
title: '[Servlet] JSON 문자열로 응답하기'
categories:
  - servlet
tags:
  - java
  - servlet
  - json
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://code.google.com/p/json-simple/](https://code.google.com/p/json-simple/)

서블릿에서 JSON 형태의 문자열을 클라이언트로 전달하는 방법 정리.

## 문자열로 응답

```java
import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

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

        String jsonTxt = "{\"code\":\"200\", \"msg\":\"success\"}";
        req.setAttribute("data", jsonTxt);
        req.getRequestDispatcher("/result.jsp").forward(req, resp);
    }
}
```

JSON 기본구조 - `{ "이름" : "값" }` 에 맞는 문자열을 전달한다.

```html
<script>
  $(document).ready(function() {
    ob1 = $.parseJSON('${data}');
    ob2 = ${data};
  });

  // non-jquery
  window.onload = function() {
    ob3 = JSON.parse('${data}');
  }
</script>
```

클라이언트는 전달받은 JSON을 문자로 취급(ob1)하거나 전달받은 그대로 객체로써 다루는 방법(ob2)으로 처리할 수 있다. 여기서 사용한 JSON.parse는 파폭, 크롬에서만 기본으로 지원되며 익스9는 라이브러리를 추가해야 한다.

![](/images/response-json-1.png)

JSON 배열을 의미하는 String 리터럴은 다음과 같은 형식으로 작성한다:

```java
"[{\"code\":\"200\", \"msg\":\"success\"}, {\"code\":\"404\", \"msg\":\"page not found\"}]"
```

만약 맵이나 컬렉션 타입을 JSON으로 파싱하고 싶다면 [jackson.ObjectMapper](http://noritersand.tistory.com/240)를 사용한다.

EL이 있어 아무도 쓰지 않는 스크립릿으로는 다음처럼 쓴다:

```html
<script>
  $(document).ready(function() {
    ob = <%=request.getAttribute("data")%>;
  });
</script>
```

## org.json.simple 패키지 이용

### JSONObject

```java
protected void process(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
    JSONObject json = new JSONObject();
    json.put("code", "200");
    json.put("msg", "success");

    req.setAttribute("data", json);
    req.getRequestDispatcher("/result.jsp").forward(req, resp);
}
```

JSONObject는 HashMap을 상속받은 클래스다. 따라서 `put()` 메서드로 프로퍼티를 추가한다.

```html
<script>
  $(document).ready(function() {
    ob1 = '${data}';
    ob2 = ${data};
  });
</script>
```

![](/images/response-json-2.png)

### JSONArray

JSON Package는 JSON객체를 배열로 다룰 수 있는 JSONArray 클래스를 지원한다. JSONArray는 ArrayList를 상속받은 클래스다.

```java
protected void process(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
    JSONArray jsonList = new JSONArray();

    JSONObject jsonOb = new JSONObject();
    jsonOb.put("code", "200");
    jsonOb.put("msg", "success");
    jsonList.add(jsonOb);

    jsonOb = new JSONObject();
    jsonOb.put("code", "404");
    jsonOb.put("msg", "page not found");
    jsonList.add(jsonOb);

    req.setAttribute("data", jsonList);
    req.getRequestDispatcher("/result.jsp").forward(req, resp);
}
```

전달된 JSONArray 객체를 for문으로 처리해봤다.

```html
<script>
  $(document).ready(function() {
    list = ${data};

    for (var loop = 0; loop < list.length; loop++) {
      console.log('code : ' + list[loop].code);
      console.log('msg : ' + list[loop].msg);
    }
  });
</script>
```

![](/images/response-json-3.png)

## AJAX로 응답하기

비동기 요청에 JSON으로 응답하는 방법을 알아보자. 이 예시에선 jQuery의 `ajax()`를 사용했다.

```html
<script>
  function getSome() {
    $.ajax({
      type: "GET",
      url: "/test.do",
      dataType: "json",
      success: function(data) {
        ob = data;
      }
    });
  }
</script>
```

문자열로 직접 응답하는 경우 다음과 같다:

```java
protected void process(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
    PrintWriter writer = resp.getWriter();

    String jsonTxt = "{\"code\":\"200\", \"msg\":\"success\"}";
    writer.print(jsonTxt);
}
```

만약 org.json.simple 패키지의 JSONObject, JSONArray 객체를 사용한다면 다음처럼 가능하다:

```java
protected void process(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
    PrintWriter writer = resp.getWriter();

    JSONArray jsonList = new JSONArray();

    JSONObject jsonOb = new JSONObject();
    jsonOb.put("code", "200");
    jsonList.add(jsonOb);

    jsonOb = new JSONObject();
    jsonOb.put("msg", "success");
    jsonList.add(jsonOb);

    writer.print(jsonList);

    System.out.println(jsonList); // [{"code":"200"},{"msg":"success"}]
}
```

이 것은 JSONObject와 JSONArray의 toString 메서드가 JSON 형태로 프로퍼티를 출력하도록 되어 있기 때문에 가능한 방법이다.
