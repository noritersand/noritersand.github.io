---
layout: post
date: 2020-04-13 09:49:00 +0900
title: '[JavaScript] Object와 Object.prototype'
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

- [\[MDN\] Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [\[MDN\] Object prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)

## 개요

`Object`와 `Object.prototype` 차이, 주요 메서드 등을 정리함.

## Object

`Object`는 `Object.prototype`의 생성자 함수이면서 동시에 객체 관련 유틸성 메서드를 제공하는 표준 내장 객체다. 프로토타입이 아니므로 `Object`의 프로퍼티와 메서드는 상속되지 않는다.

### [Object.getOwnPropertyDescriptor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor), [Object.getOwnPropertyDescriptors()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)

객체의 모든 프로퍼티 설명자 혹은 특정 프로퍼티의 설명자를 반환한다. 설명자란 해당 프로퍼티가 어떻게 설정(쓰기 가능한지, 열거 가능한지 등등)되어 있는지를 알려주는 객체다.

객체 자신이 소유한 프로퍼티가 아니면 반환되지 않는다. 가령 새로 정의한 객체`{ a: 1 }`는 소유한 프로퍼티가 `a`만 있으므로 `toString`의 프로퍼티 설명자는 안나온다.

```js
var o = { a: 1 };
Object.getOwnPropertyDescriptors(o);
// Object { a: Object { value: 1, writable: true, enumerable: true, … } }

Object.getOwnPropertyDescriptor(o, 'a');
// Object { value: 1, writable: true, enumerable: true, configurable: true }

Object.getOwnPropertyDescriptor(o, 'toString'); // undefined

Object.getOwnPropertyDescriptor(Object.prototype, 'toString');
// Object { value: toString(), writable: true, enumerable: false, configurable: true }

Object.getOwnPropertyDescriptors(String.prototype);
// Object { length: {…}, toString: {…}, valueOf: {…}, toLowerCase: {…}, toUpperCase: {…}, charAt: {…}, charCodeAt: {…}, substring: {…}, padStart: {…}, padEnd: {…}, … }
```

### [Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

객체에 새로운 프로퍼티를 정의하거나 이미 존재하는 프로퍼티를 수정하는 메서드.

```js
Object.defineProperty(Object.prototype, 'name', {
  value: 'waldo', writable: false, enumerable: true
});

Object.getOwnPropertyDescriptor(Object.prototype, 'name');
// { value: "waldo", writable: false, enumerable: true, configurable: false }
```

### [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create#Classical_inheritance_with_Object.create)

### [Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

## Object.prototype

모든 객체의 조상, 최상위 프로토타입인 `Object` 프로토타입.

### [Object.prototype.hasOwnProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

## 꼐속...
