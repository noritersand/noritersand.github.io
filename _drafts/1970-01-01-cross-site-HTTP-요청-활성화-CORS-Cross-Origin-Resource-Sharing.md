---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'cross-site HTTP 요청 활성화(CORS, Cross-Origin Resource Sharing)'
categories:
  - web
tags:
  - http
  - cors
---

#### 참고한 글
- [https://www.w3.org/TR/cors/](https://www.w3.org/TR/cors/)
- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

TODO
- top window와 iframe의 도메인이 다를때 크로스 사이트(혹은 크로스 오리진) 상태이며 이 때 parent와 child간 스크립트 호출은 차단된다. (크롬에선 'caught DOMException: Blocked a frame with origin "http://localhost:8081" from accessing a cross-origin at <anonymous>:1:15' 라고 뱉음)

- CORS로 위 현상이 해소되는지 확인할 것

- opener와 새 창도 iframe과 같은 현상이 있음.
