---
layout: post
date: 2013-08-16 04:13:00 +0900
title: '[Servlet] forward, redirect'
categories:
  - servlet
tags:
  - java
  - servlet
  - forward
  - redirect
---

* Kramdown table of contents
{:toc .toc}

## Redirect

클라이언트의 요청을 처리 한 후, 컨테이너는 `sendRedirect()`가 호출되면 브라우저에 응답을 보낸다. 이 응답에는 브라우저가 웹 컨테이너의 응답을 받은 후 다시 요청을 보낼 새로운 URL을 포함한다. 여기서 새로 부여받은 URL로 브라우저에서 완전히 새롭게 요청하기 때문에 이전 request에 저장되어 있는 객체는 소멸된다.

#### JSP에서

```java
<%
    String cp=response.getContextPath();
    response.sendRedirect(cp+"/bbs/list.do");
%>
```

#### servlet에서

```java
String cp=response.getContextPath();
response.sendRedirect(cp+"/bbs/list.do");
```

## Forward

요청을 forward 할 때 해당 요청은 서버의 다른 자원(서블릿 또는 JSP)에 전달된다. 이때 클라이언트에 알리지 않는다. 이런 방식의 처리는 웹 컨테이너 내부에서 발생하고 클라이언트는 알 수 없다. forward는 redirect과 다르게 객체를 요청에 담고 해당 요청을 사용할 다음 자원에 전송한다.

#### JSP에서

```html
<jsp:forward page="/WEB-INF/jsp/bbs/list.jsp"/>
```

#### servlet에서

```java
RequestDispatcher rd = request.getRequestDispatcher("/WEB-INF/jsp/bbs/list.jsp");
rd.forward(request, response);
```

forward 사용 시 루트 경로는 웹루트와 동일하다.
