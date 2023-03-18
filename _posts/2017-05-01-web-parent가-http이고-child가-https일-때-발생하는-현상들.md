---
layout: post
date: 2017-05-01 21:13:00 +0900
title: '[WEB] parent가 http이고 child가 https일 때 발생하는 현상들'
categories:
  - web
tags:
  - web
  - iframe
  - http
  - https
  - debugging-log
---

* Kramdown table of contents
{:toc .toc}

이 글에서...

- 'parent'는 탑(혹은 메인) 프레임, body 내에 iframe을 갖는 페이지를 뜻한다.
- 'child'는 parent의 body 내에 있는 iframe을 뜻한다.

## child나 parent에서 서로의 자바스크립트 객체나 DOM 객체에 접근 불가

동일 출처 정책 위반으로 브라우저가 차단함. parent와 child간 상호작용은 거의 불가능하다고 봐야 한다.

```js
// 교차 출처인 윈도우의 프로퍼티(변수, 함수, 브라우저 객체 등)에 접근 불가
window.frames[0].history; // SecurityError: Permission denied to access on cross-origin object
window.frames[0].foo; // SecurityError: Permission denied to access on cross-origin object
window.frames[0].myFn; // SecurityError: Permission denied to access on cross-origin object
window.frames[0].myFn(); // SecurityError: Permission denied to access on cross-origin object
```

## location을 바꿀 수는 있지만 접근은 불가능

parent가 child의 주소를 바꾸는건 가능하지만 child의 현재 주소가 뭔지를 얻는건 불가능하다. 이것은 반대의 경우(child에서 parent로)도 마찬가지다.

```js
// 교차출처인 윈도우의 주소를 바꿀 수는 있지만
window.frames[0].location.href = 'http://tistory.com';
// 현재 주소가 뭔지 알아내는건 불가능
var childUrl = window.frames[0].location.href; // SecurityError: Permission denied to access on cross-origin object
```

## 혼합된 콘텐츠

child 내에서 http 콘텐츠를 불러올 수 없거나 경고 메시지가 발생한다. 이미지는 경고로 끝나고 css나 js는 차단된다. child내의 또 다른 iframe이 있을 때도 https 콘텐츠가 아니면 차단된다.

## 참고

[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)로는 위 제한들을 [우회할 수 없다](http://stackoverflow.com/questions/25098021/securityerror-blocked-a-frame-with-origin-from-accessing-a-cross-origin-frame).

이럴 때 쓰라고 제한된 조건 하에서 통신이 가능한 [Window.postMessage](http://noritersand.tistory.com/655) 메서드가 있다.
