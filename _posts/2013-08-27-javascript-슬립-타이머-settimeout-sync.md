---
layout: post
date: 2013-08-27 15:11:00 +0900
title: '[JavaScript] 슬립 타이머(setTimeout sync 버전)'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - timer
  - sleep
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


non-blocking 전역함수 `setTimeout()`의 blocking 버전이다:

```js
function sleep(callback, millisec) {
  console.log('Good night.');
  var startTime = new Date().getTime();
  while (new Date().getTime() < startTime + millisec);
  callback();
}

sleep(function() {
  console.log('Where am I?');
}, 3000);
console.log('Calm down, it\'s hospital.');

// Good night.
// Where am I?
// Calm down, it's hospital.
```
