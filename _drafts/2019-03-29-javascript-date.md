---
layout: post
date: 2019-03-29 00:00:00 +0900
title: '[JavaScript] Date'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - date
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date)

Standard built-in Objects: Date

TODO

## new Date()

```js
const { log } = console;

let a = new Date('2018-01-01T12:24:48Z');
let b = new Date('2018-01-01 12:24:48'); // 얘는 IE, 사파리에서 문제 있음.
let c = new Date('2016-02-05T09:00:00.000+09:00');
let d = new Date('2011-12-30');
let e = new Date('2011/12/30');

log(a.getTime()); // 1514809488000
log(b.getTime()); // 1514777088000
log(c.getTime()); // 1454630400000
log(d.getTime()); // 1325203200000
log(e.getTime()); // 1325170800000

let err1 = new Date('20111230');
let err2 = new Date('30-12-2011');

log(err1.getTime()); // NaN
log(err2.getTime()); // NaN
```

## Date.UTC()

```js
new Date(Date.UTC(2019, 4, 12, 11, 45))
// Sun May 12 2019 20:45:00 GMT+0900 (한국 표준시)
```
