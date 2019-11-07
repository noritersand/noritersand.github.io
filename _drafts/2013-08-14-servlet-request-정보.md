---
layout: post
date: 2013-08-14 23:47:00 +0900
title: '[Servlet] Request 정보'
categories:
  - servlet
tags:
  - java
  - servlet
  - request
---

* Kramdown table of contents
{:toc .toc}

## HTTP method 확인

```java
String method = request.getMethod();
```

## ContextPath

```java
String cp = request.getContextPath();
//: /study
```

## 요청 URL

```java
String url = request.getRequestURL().toString();
//http://localhost:9090/study/0222/test3_ok.jsp

String path = request.getScheme() + "://"
    +  request.getServerName() + ":"
    +  request.getServerPort()
    +  request.getContextPath(); //http://localhost:9090/study
```

## URL에서 스키마, 서버이름, 포트번호를 제외한 나머지 주소와 파라미터

```java
String excludeHost = request.getRequestURI();
//: /study/0222/test3_ok.jsp
```

## https와 같은 보안 채널의 사용 여부 true/false

```java
request.isSecure()
```

## local의 기본 정보 (서버의 정보)

```java
request.getLocalAddr();
request.getLocalName();
request.getLocalPort();
```

## 클라이언트의 ip 및 포트 정보

```java
Remote IP: <%= request.getRemoteAddr()%>
Remote Host: <%= request.getRemoteHost()%>
Remote Port: <%= request.getRemotePort()%>

<%
    String ip = request.getHeader("HTTP_X_FORWARDED_FOR");
    if (ip == null || ip.length() == 0 || ip.toLowerCase().equals("unknown")) {
        ip = request.getHeader("REMOTE_ADDR");
    }
    if (ip == null || ip.length() == 0 || ip.toLowerCase().equals("unknown")) {
        ip = request.getRemoteAddr();
    }
%>
```

## 지역 정보

```java
request.getLocale();
```

## 사용하는 프로토콜

```java
request.getProtocol(); // HTTP/1.1, '프로토콜/메이저버전.마이너버전'
```

## session id

```java
request.getRequestedSessionId(); // session id
request.getSession().getId(); // session id
request.isRequestedSessionIdFromCookie(); // session id가 쿠키로 제공되었는지 여부
request.isRequestedSessionIdFromURL(); // session id가 URL의 일부로 제공되었는지 여부
request.isRequestedSessionIdValid(); // session이 아직 유효한지
```

## Request 객체를 통해서 쿠키 전체 보는 방법

```java
Cookie cookies[] = request.getCookies();
for(int i=0 ; i<cookies.length ; i++) {
    String name = cookies[i].getName();
    String value = cookies[i].getValue();
    logger.debug(name + " : " + value);
}
```

## 폼 데이터 받기

```java
Enumeration eParam = request.getParameterNames();
while (eParam.hasMoreElements()) {
    String pName = (String)eParam.nextElement();
    String pValue = request.getParameter(pName);
    logger.debug(pName + " : " + pValue);
}
```

## request body로 넘어오는 데이터

```java
DataInputStream dis = new DataInputStream(request.getInputStream());
String str;

while ((str = dis.readLine()) != null) {
    logger.debug(new String(str.getBytes("ISO-8859-1"), "euc-kr")+"<br/>");
    // utf-8로 전송된 한글은 깨짐
}
```

## Request.attribute

### attribute 전체 확인

```java
Enumeration eAttr = request.getAttributeNames();

while (eAttr.hasMoreElements()) {
    String aName = (String)eAttr.nextElement();
    String aValue = (String)request.getAttribute(aName);
    logger.debug(aName + " : " + aValue);
}
```

### forward

```java
request.getAttribute("javax.servlet.forward.request_uri");
request.getAttribute("javax.servlet.forward.context_path");
request.getAttribute("javax.servlet.forward.servlet_path");
request.getAttribute("javax.servlet.forward.path_info");
request.getAttribute("javax.servlet.forward.query_string");
```

### include

```java
request.getAttribute("javax.servlet.include.request_uri");
request.getAttribute("javax.servlet.include.query_string");
request.getAttribute("javax.servlet.include.path_info");
request.getAttribute("javax.servlet.include.servlet_path");
request.getAttribute("javax.servlet.include.context_path");
```
