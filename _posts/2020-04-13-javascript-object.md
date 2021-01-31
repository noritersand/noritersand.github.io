---
layout: post
date: 2020-04-13 09:49:00 +0900
title: '[JavaScript] Object'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - Object
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)

`Object.prototype`의 생성자 함수이면서 동시에 객체 관련 유틸성 메서드를 제공하는 표준 내장 객체. `Object`의 프로퍼티와 메서드는 상속되지 않는다. (프로토타입이 아니니께)

## Object

### [Object.getOwnPropertyDescriptor()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

```js
Object.getOwnPropertyDescriptor(Object.prototype, 'toString')
// {writable: true, enumerable: false, configurable: true, value: ƒ}

Object.getOwnPropertyDescriptor({ a: 1 }, 'a')
// Object { value: 1, writable: true, enumerable: true, configurable: true }
```

### [Object.defineProperty()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

객체에 새로운 속성을 정의하거나 이미 존재하는 속성을 수정하는 메서드.

```js
Object.defineProperty(Object.prototype, 'name', {
    value: 'waldo', writable: false, enumerable: true
});

Object.getOwnPropertyDescriptor(Object.prototype, 'name');
// { value: "waldo", writable: false, enumerable: true, configurable: false }
```


### [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create#Classical_inheritance_with_Object.create)

## Object.prototype

### [Object.prototype.hasOwnProperty()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

## 꼐속...
