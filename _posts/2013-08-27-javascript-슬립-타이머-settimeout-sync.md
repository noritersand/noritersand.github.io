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

```js
function sleep(callback, millisec) {
  console.log("good night");
  var startTime = new Date().getTime();
  while (new Date().getTime() < startTime + millisec);
  callback();
}

sleep(function() {
  console.log("... where am i?");
}, 3000);
```
