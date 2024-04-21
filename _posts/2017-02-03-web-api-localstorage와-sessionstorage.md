---
layout: post
date: 2017-02-03 13:42:00 +0900
title: '[Web API] localStorage와 sessionStorage'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - html-standard
  - localstorage
  - sessionstorage
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Window.localStorage \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [Window.sessionStorage \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)


## localStorage

localStorage는 브라우저 전체에서 공유되는 저장공간이다. 여러 창(혹은 탭)끼리도 같은 저장공간을 사용한다.


## sessionStorage

sessionStorage는 localStorage와 다르게 Window마다 분리되는 저장공간이다. 브라우저의 다른 창(혹은 탭)에서는 Window가 다르므로 sessionStorage를 공유하지 않는다. **Java Servlet에서 말하는 세션과는 다른 개념**이므로 혼동하지 말자. 브라우저를 종료하면 자동으로 초기화된다.


## 같은 호스트인지 판단하는 기준

localStorage와 sessionStorage는 호스트의 프로토콜과 포트 번호 별로 각각의 저장소에 저장한다. 그래서 HTTP와 HTTPS는 도메인이 같아도 서로 데이터를 공유할 수 없으며, 포트 번호가 다를 때도 마찬가지다.

웹 API인 `location.host`는 포트번호를 포함하는데 아무래도 이것과 관련이 있는 것 같다:

```js
console.log(location.host); // "localhost:8888"
```


## 메서드

**TODO**
