---
layout: post
date: 2019-04-01 00:00:00 +0900
title: '[WEB] CORS, Cross-Origin Resource Sharing'
categories:
  - web
tags:
  - http
  - cors
  - javascript
  - browser
---

#### 참고한 문서

- [https://www.w3.org/TR/cors/](https://www.w3.org/TR/cors/)
- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
- [https://developer.mozilla.org/en-US/docs/Glossary/CSRF](https://developer.mozilla.org/en-US/docs/Glossary/CSRF)
- [https://www.popit.kr/cors-preflight-인증-처리-관련-삽질/](https://www.popit.kr/cors-preflight-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B4%80%EB%A0%A8-%EC%82%BD%EC%A7%88/)
- [https://homoefficio.github.io/2015/07/21/Cross-Origin-Resource-Sharing/](https://homoefficio.github.io/2015/07/21/Cross-Origin-Resource-Sharing/)


## 동일 출처 원칙

자바스크립트에서는 어떤 윈도우와 또 다른 윈도우의 스킴(scheme)과 호스트명, 그리고 포트번호가 완전히 일치하지 않을때, 두 윈도우간의 상호작용을 차단한다. 이것은 [동일 출처 원칙(same origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)의 제약사항이며 출처가 다른 두 윈도우를 교차 출처(cross origin) 상태에 있다고 한다.

교차 출처인 윈도우의 변수나 함수, 즉 프로퍼티로의 접근은 대부분 차단된다고 생각하면 된다. 가령 A 윈도우에서 교차 출처인 B 윈도우의 주소를 Location을 통해 가져오려고 하면 DOMException이 발생하면서 스크립트가 중단되는 식이다. 참고로 교차 출처 상태일 때 location으로 주소를 알아낼 수는 없지만 주소를 바꿀 수는 있다.


## CORS란?

https://developer.mozilla.org/ko/docs/Web/HTTP/CORS

TODO


## 자매품 CSRF

https://developer.mozilla.org/en-US/docs/Glossary/CSRF

Cross-Site Request Forgery.


## TODO

top window와 iframe의 도메인이 다를때 크로스 사이트(혹은 크로스 오리진) 상태이며 이 때 parent와 child간 스크립트 호출은 차단된다.

```js
'caught DOMException: Blocked a frame with origin "http://localhost:8081" from accessing a cross-origin at <anonymous>:1:15' // 크롬
```

- CORS로 위 현상이 해소되는지 확인할 것
- opener와 새 창도 iframe과 같은 현상이 있음.


## cross-site HTTP 요청 활성화

TODO
