---
layout: post
date: 2016-07-04 15:59:00 +0900
title: '[JavaScript] 심볼 Symbol'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - symbol
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)


Symbol은 기본형(primitive, 원시형) 데이터 타입으로 ES2015 부터 추가되었다. 다른 기본형 타입인 Number, String, Boolean과 마찬가지로 [래퍼(wrapper) 객체](http://noritersand.tistory.com/536)가 있다.

이터레이터가 있는 모양?

꼐속...

## 객체가 iterable한지 확인

```js
function isIterable(obj) {
  if (obj == null) {
    return false;
  }
  return typeof obj[Symbol.iterator] === 'function';
}
```
