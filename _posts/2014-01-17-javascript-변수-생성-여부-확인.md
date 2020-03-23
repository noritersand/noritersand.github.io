---
layout: post
date: 2014-01-17 15:19:00 +0900
title: '[JavaScript] 변수 생성 여부 확인'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - variable
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

#### try-catch

```js
var flag;

try {
  foo;
  flag = true;
} catch (e) {
  flag = false;
}

console.log(flag);
```

#### typeof

```js
var flag;

if (typeof foo == 'undefined') {
  flag = false;
} else {
  flag = true;
}

console.log(flag);
```
