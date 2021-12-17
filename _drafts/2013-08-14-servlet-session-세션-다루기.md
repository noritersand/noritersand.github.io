---
layout: post
date: 2013-08-14 23:56:00 +0900
title: '[Servlet] Session 세션 다루기'
categories:
  - servlet
tags:
  - java
  - servlet
  - session
---

* Kramdown table of contents
{:toc .toc}

#### JSP에서 session 접근

jsp가 java로 파싱되면서 "session" 변수로 자동 생성된다.

```java
session = request.getSession(true);
String id = request.getParameter("id");
request.getSession().setAttribute("id", id);
```

#### 서블릿에서 session 접근(로그인)

request 를 통해 session 에 접근한다.

```java
HttpSession session = req.getSession(); //세션에 클라이언트의 정보를 저장

session.setMaxInactiveInterval(60*30); //세션 유효시간 : (60초*30) 후 로그아웃 //디폴트 30분

session.setAttribute("userId", dto.getUserId());
session.setAttribute("userName", dto.getUserName());
resp.sendRedirect(cp);
```

#### EL/JSTL - 서블릿이 전달한 session 출력

```java
${sessionScope.dto.userId} //바로 출력, null이면 나오지 않는다.

<c:if test="${empty sessionScope.userId}">
    로그아웃 상태 시 표시할 태그
</c:if>

<c:if test="${not empty sessionScope.userId}">
    로그인 상태 시 표시할 태그
</c:if>

//expression으로 출력하는 경우..
<%
String birth = (String)session.getAttribute("birth");
MemberDTO dto = (MemberDTO)session.getAttribute("member");
%>
<%=birth%><br/>
<%=dto.getUserId()%><br/>
<%=dto.getUserName()%><br/>
```

#### 로그인 확인

```java
HttpSession session = req.getSession();
String userId = (String)session.getAttribute("userId");

//로그인상태 확인, 세션에 "userId"가 없다면 로그아웃으로 간주하는 방식
if (userId == null) {
    forward(req, resp, "/memView/login.jsp");
    return;
}
forward(req, resp, "/imgView/created.jsp");
```

#### session 삭제(로그아웃)

```java
//세션에 저장된 정보를 삭제한다.
session.removeAttribute("userId");
session.removeAttribute("userName");

//세션의 모든 정보를 삭제하고 현재 세션을 무효화
session.invalidate();
```
