---
layout: post
date: 2013-08-14 23:24:00 +0900
title: 'Servlet: Request header 정보'
categories:
  - java
  - servlet
tags:
  - java
  - servlet
  - request
  - header
---

* Kramdown table of contents
{:toc .toc}

```java
String referer = request.getHeader("Referer"); //: http://localhost:9090/study/0222/test3.jsp
```

- `Accept`: 브라우저나 기타 클라이언트가 다룰 수 있는 MIME 형식들을 지정한다.
- `Accept-Charset`: 폼 데이터로 전송된 값의 인코딩 문자셋(encoding character set)을 의미한다.
- `Accept-Encoding`: 클라이언트가 원하는 인코딩 방식들을 의미한다. 문서를 네트웍 너머로 보내기에 적합한 형태로 변환하는것을 의미한다.
- `Accept-Language`: 클라이언트가 원하는 기본 언어를 의미한다.
- `Authorization`: 헤더는 패스워드로 보호된 웹 페이지에 접근하려 하는 클라이언트가 자신의 신원 정보를 보내는 용도로 쓰인다.
- `Cache-Control`: 프록시 서버에서의 캐시 설정을 의미한다.
- `Connection`: 클라이언트가 영속적 HTTP 연결을 처리할 수 있는지의 여부를 뜻한다.
- `Content-Length`: post만 해당하는 것으로, post 데이터에 담긴 데이터의 크기를 의미한다.
- `Content-type`: 주로 응답에서 쓰이지만, 클라이언트가 post나 put을 이용해서 어떤 문서를 첨부했다면 클라이언트 요청 안에 포함되기도 한다.
- `Cookie`: 서버가 브라우저에게 보냈던 쿠키를 다시 서버로 돌려 보낼 때 쓰인다.
- `Expect`: 클라이언트가 바라는 서버의 행동이 어떤 것인지를 의미하는 헤더로 별로 쓰이지는 않는다.
- `From`: HTTP 요청에 대해 책임이 있는 사람의 전자 우편주소이다.
- `Host`: URL안에 호스트와 포트 번호로 브라우저는 반드시 이 헤더를 포함해야함. 브라우저가 원하는 구체적인 자원을 정확하게 돌려주기 위한 용도로 쓰임.
- `If-Match`: put요청에 해당하는 헤더
- `If-Modified-Since`: 어떤 페이지의 최종 수정일시가 특정항 일시 이후인 경우만 그 페이지를 돌려 받기원하는 경우 이헤더에 그 일시를 헤더에 설정해서 요청한다.
- `If-None-Match`: If-Match와 받대
- `If-Range`: 다운로드 관리자의 이어받기 기능에서 주로 이용된다. 어떤 하나의 문서의 일부분만을 가지고 있을 때 수정 일시를 지정해서 그 일시 이후에 변한 것이 없으면 이미 가지고 있는 부분 이외의것만 받게하고 아니면 문서 전체를 받는 용도로 쓰인다.
- `If-Unmodified-Since`: If-Modified-Since 반대
- `Pragma`: 헤더 값이 no-catch이면 프록시 서버 또는 하나의 프로시로서 작동하는 서블릿은 캐시에 복사본이 있다고 해도 서버에서 처리한 response를 전달함
- `Range`: If-Range 와 비슷하다.
- `Referer`: 이전 url정보를 얻을 때 사용. 원래 의미대로라면 referrer가 맞다. referer가 된것은 오타 때문이라고 한다.
- `Upgrade`: 클라이언트가 서버에게 HTTP1.1 이상의 어떠한 프로토콜을 지정하는 용도로 쓰인다.
- `User-Agent`: 브라우저의 소프트웨어 종류와 버전을 의미한다.
- `Via`: 요청이 거쳐간 중간 사이트들을 의미한다 주로 게이트웨이나 프록시 서버가 설정한다.
- `Warning`: 클라이언트가 캐싱이나 내용 변환 관련 에러에 대해 경고 할 때 쓰이는 헤더로 자주 쓰이지 않는다.

## 모든 header 정보 구하기

```java
Enumeration<String> headers = request.getHeaderNames();
while (headers.hasMoreElements()) {
    String name = headers.nextElement();
    String value = request.getHeader(name);
    log.debug(name + " : " + value);
}
```
