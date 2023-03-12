---
layout: post
date: 2019-05-08 13:55:00 +0900
title: '[web] HTTP 요청 메서드 종류 request methods'
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

- [https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)
- [\[MDN\] HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [RFC 5789, section 2: Patch method](https://tools.ietf.org/html/rfc5789)

HTTP 요청 메서드 간단 정리 글.

2019-05-08 기준, 총 아홉 종류의 메서드가 있다:

- OPTIONS
- GET
- HEAD
- POST
- PUT
- DELETE
- TRACE
- CONNECT
- PATCH


## OPTIONS

URL이나 서버에 허용된 통신 옵션을 요청한다. 응답 헤더를 통해 어떤 HTTP 메서드를 지원하는지와 CORS이 가능한지를 확인할 때 사용한다.

CORS 스펙에서는 따르면 브라우저는 GET 메서드를 제외한 다른 메서드에서 사전 요청인 preflight를 OPTIONS 메서드로 요쳥해야 한다고 함.

```bash
# OPTIONS HTTP 요청하며 응답 헤더 출력
curl -i -X OPTIONS https://example.org
```


## GET

> The GET method means retrieve whatever information (in the form of an entity) is identified by the Request-URI. If the Request-URI refers to a data-producing process, it is the produced data which shall be returned as the entity in the response and not the source text of the process, unless that text happens to be the output of the process.

조회.


## HEAD

> The HEAD method is identical to GET except that the server MUST NOT return a message-body in the response. The metainformation contained in the HTTP headers in response to a HEAD request SHOULD be identical to the information sent in response to a GET request. This method can be used for obtaining metainformation about the entity implied by the request without transferring the entity-body itself. This method is often used for testing hypertext links for validity, accessibility, and recent modification.


## POST

> The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line. POST is designed to allow a uniform method to cover the following functions:
> - Annotation of existing resources;
> - Posting a message to a bulletin board, newsgroup, mailing list, or similar group of articles;
> - Providing a block of data, such as the result of submitting a form, to a data-handling process;
> - Extending a database through an append operation.

신규 데이터 등록


## PUT

> The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI. If a new resource is created, the origin server MUST inform the user agent via the 201 (Created) response. If an existing resource is modified, either the 200 (OK) or 204 (No Content) response codes SHOULD be sent to indicate successful completion of the request. If the resource could not be created or modified with the Request-URI, an appropriate error response SHOULD be given that reflects the nature of the problem. The recipient of the entity MUST NOT ignore any Content-* (e.g. Content-Range) headers that it does not understand or implement and MUST return a 501 (Not Implemented) response in such cases.

등록된 데이터 변경 시 사용하는 메서드.

### POST와 PUT의 차이

앞서 언급했듯이 POST는 신규 생성, PUT은 업데이트에 사용한다. 그리고 멱등하냐 그렇지 않냐의 차이가 있다:

> All safe methods are idempotent, as well as PUT and DELETE. The POST method is not idempotent.
> https://developer.mozilla.org/en-US/docs/Glossary/Idempotent

> idempotent
> 1. 멱등(冪等)의 2. 멱등원(元)
>
> [네이버 영어사전: idempotent](https://en.dict.naver.com/#/entry/enko/df125e744fd141d89c385d7b1b5063c1)

멱등성이란 어떠한 연산이나 요청을 여러번 수행해도 한 번만 수행한 것과 동일한 결과가 유지되는 것을 말한다.

PUT은 멱등하다(혹은 멱등성을 가진다). 특정 리소스의 업데이트는 반복해서 수행해도 결과가 같다. 반면 POST는 요청을 수행할 때마다 새로운 리소스가 생성된다. 따라서 POST는 멱등하지 않다.


## DELETE

> The DELETE method requests that the origin server delete the resource identified by the Request-URI. This method MAY be overridden by human intervention (or other means) on the origin server. The client cannot be guaranteed that the operation has been carried out, even if the status code returned from the origin server indicates that the action has been completed successfully. However, the server SHOULD NOT indicate success unless, at the time the response is given, it intends to delete the resource or move it to an inaccessible location.

삭제

스펙은 GET과 비슷해서 리퀘스트 바디를 쓸 수 없다. (아예 안되는 건 아닌 모양) 따라서 삭제하려는 데이터의 식별값은 query-string으로 던져야 한다.


## TRACE

> The TRACE method is used to invoke a remote, application-layer loop- back of the request message. The final recipient of the request SHOULD reflect the message received back to the client as the entity-body of a 200 (OK) response. The final recipient is either the


## CONNECT

> This specification reserves the method name CONNECT for use with a proxy that can dynamically switch to being a tunnel


## PATCH

리소스의 부분적 수정 시 사용하는 메서드. (PUT 메서드는 리소스의 전체 수정을 의미한다.)

PUT 메서드와 달리 반드시 멱등할 필요는 없다고 함. PATCH는 PUT처럼 모든 것을 덮어쓰는게 아닐 수도 있기 때문이라고... 그러니까 개발하기에 따라서 멱등하기도, 멱등하지 않기도 한다는 말. 

\* 멱등하지 않다는 것은 요청마다 다른 결과를 만들거나, 새 리소스가 생긴다는 뜻.
