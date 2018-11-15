---
layout: post
date: 2017-02-03 13:42:00 +0900
title: 'JavaScript: Promise'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - promise
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [http://www.html5rocks.com/ko/tutorials/es6/promises/](http://www.html5rocks.com/ko/tutorials/es6/promises/)

#### 브라우저 지원

- Chrome 32 이상
- Edge
- FireFox 29 이상
- Opera 19 이상
- safari 7.1 이상
- IE 모든 버전에서 사용 불가

나불나불

#### 예시 1

[퍼온거임](http://www.ubus.kr/kiss2me/start.php?id=TG1&page=9&divpage=1&sn=off&ss=on&sc=off&select_arrange=headnum&desc=asc&no=2799)

pure 자바스크립트로 구현(이라기보단 흉내)한 promise 패턴...인가? 아마 promise 패턴의 이해를 돕기위한 예시 같다.

```js
function MyDefferd() {
  // 실행순서: 3
  // do nothing
}

MyDefferd.prototype.done = function(func) {
  // 실행순서: 4
  this.fn = func; // fn에 함수 할당
};

function Deffer(delay) {
  // 실행순서: 2
  var deff = new MyDefferd();

  alert('작업 시작!');

  setTimeout(function() {
    // 실행순서: 5
    if (typeof(deff.fn) == 'function') {
      deff.fn(); // 4번에서 할당된 함수 실행
    }
  }, delay*1000);

  return deff;
}

// 실행순서: 1
Deffer(3).done(function() {
  // 실행순서: 6
  alert('작업 끝!');
});
```
