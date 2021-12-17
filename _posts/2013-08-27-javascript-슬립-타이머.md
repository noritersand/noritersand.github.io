---
layout: post
date: 2013-08-27 15:11:00 +0900
title: '[JavaScript] 슬립 타이머'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - timer
  - sleep
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```js
function sleep(foo, millisec) {
  console.log("good night");
  var startTime = new Date().getTime();
  while (new Date().getTime() < startTime + millisec);
  foo();
}

sleep(function() {
  console.log("... where am i?");
}, 3000);
```
