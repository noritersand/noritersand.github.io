---
layout: post
date: 2013-08-14 23:19:00 +0900
title: 'Servlet: Cookie 쿠키 다루기'
categories:
  - servlet
tags:
  - java
  - servlet
  - cookie
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- http://tomcat.apache.org/tomcat-7.0-doc/servletapi/javax/servlet/http/Cookie.html

#### 쿠키설정

```java
//쿠키는 문자열만 설정할 수 있으며 한글은 반드시 인코딩해야한다.
Cookie c1 = new Cookie("name", URLEncoder.encode("홍길동", "utf-8"));
Cookie c2 = new Cookie("age", "10");
Cookie c3 = new Cookie("tel", "000-0000");
Cookie c4 = new Cookie("addr", "seoul");

//쿠키의 유효시간(기본값은 -1로 브라우저가 닫히면 사라진다.)
c3.setMaxAge(60);//초단위
c4.setMaxAge(0);//생성과 동시에 제거됨

//클라이언트에 쿠키 생성
response.addCookie(c1);
response.addCookie(c2);
response.addCookie(c3);
response.addCookie(c4);
```

#### 쿠키가져오기

```java
Cookie[] cc = request.getCookies();
if (cc!=null) {
    out.print("쿠키이름 : 쿠키값<br/>");
    for (Cookie c : cc) {
        out.print(c.getName() + " : ");
        String s = c.getValue();

        if(c.getName().equals("name")) //출력할 한글 디코딩
            s = URLDecoder.decode(s, "utf-8");

        out.print(s + "<br/>");
    }
}
// setMaxAge(0)인 c4는 사라져서 보이지 않는다.
```

#### JSESSIONID 를 제외한 쿠키 출력

```java
Cookie[] cc = request.getCookies();
if (cc != null) {
    for(Cookie c : cc) {
        String name = c.getName();
        String value = URLDecoder.decode(c.getValue(), "utf-8");

        if(name.startsWith("name")) {
            out.print(value + "<br/>");
        }
    }
}

/*
 * 쿠키의 이름이 "name"으로 시작하면 모두 출력
 * 단 이 경우엔 쿠키의 이름을 "name" + "String" 형태로 만들어야한다. name1, name2...
 *
 * JSESSIONID는 브라우저로 웹페이지 로딩 시에 쿠키[0]에 저장되지만
 * 이후 브라우저를 재실행하면(쿠키가 남아있을 경우) 새로 생성한 쿠키보다 뒤에 위치한다.
 * ex) 생성한 쿠키가 한 개라면 생성 직 후엔 [0], 브라우저 재실행 후엔 [1]에 위치한다.
 */
```

#### 쿠키제거

```java
Cookie c1 = new Cookie("name", null);
c1.setMaxAge(0);
response.addCookie(c1);
```
