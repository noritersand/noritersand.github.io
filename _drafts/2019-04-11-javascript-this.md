---
layout: post
date: 2019-04-11 11:15:00 +0900
title: '[JavaScript] this'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - this
---

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

## this란 무엇인가

블라블라

## 자바스크립트 객체 메서드 내에서 this

```js
var owner = {
  fn: function() {
    return this;
  }
};

owner === owner.fn();
```

여기서 `this`는 함수를 소유한 객체다.
