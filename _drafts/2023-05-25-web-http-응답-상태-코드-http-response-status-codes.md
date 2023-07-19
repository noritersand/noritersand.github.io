---
layout: post
date: 2023-05-25 18:21:12 +0900
title: '[WEB] HTTP 응답 상태 코드'
categories:
  - web
tags:
  - http
  - request
  - method
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [RFC 9110 HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html)
- [\[MDN\] HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)


## 개요

어쩌구


## 100 - 199

정보성


## 200 - 299

성공


## 300 - 399

리디렉션

### 302 Found

임시로 변경된 주소로 리디렉션하라는 상태 코드

### 307 Temporary Redirect

임시로 변경된 주소로 리디렉션한다. 302와 다른 점은 원 요청의 바디와 메서드를 변경하지 않는다는 것이다.


## 400 - 499

요청을 제대로 처리하지 못했으며, 오류의 원인이 클라이언트에게 있음을 알리는 코드 범위.

여기서 클라이언트라는 게 실사용자만을 의미하는 건 아니다. 오히려 개발자가 만든 클라이언트용 앱에서 문제가 발생했을 가능성이 더 크다.


## 500 - 599

4xx와 비슷하지만, 이 쪽은 오류의 원인이 서버에 있을 때를 의미한다.

### 503 Service Unavailable

서버가 요청을 처리할 준비가 되지 않았음을 의미한다. 일반적으로 소켓은 열려있으나 서버 애플리케이션이 죽어있을 때, 혹은 과부하로 다운됐을 때 발생한다.

서버는 응답에 `Retry-After` 헤더를 포함 시켜 예상 복구 시간을 알릴 수 있다.

MDN에선 이 응답을 내보낼 때 캐시를 사용하지 않도록 주의하라고 한다. 503 상태는 일시적인 경우가 많고 캐싱이 필요 없기 때문이다.

