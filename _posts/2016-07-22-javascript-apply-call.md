---
layout: post
date: 2016-07-22 19:00:10 +0900
title: '[JavaScript] apply(), call()'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - apply
  - call
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Function.prototype.apply()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [\[MDN\] Function.prototype.call()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

## 개요

`apply()`와 `call()`은 `Function`의 인스턴스 메서드(프로토타입의 메서드)다. 따라서 모든 `Function` 객체, 즉 함수는 `apply()`와 `call()`을 호출할 수 있다. 이 메서드들은 함수를 특별하게 호출하기 위해 사용한다.

예를 들어 `NodeList` 타입은 반복이 가능한 array-like 타입이지만 `Array`가 아니다. 그래서 `Array`가 가진 모든 메서드를 쓸 수 없는 문제가 있는데 이 때 `apply()`나 `call()`을 쓴다:

```js
var nodeList = document.querySelectorAll('div');
console.log(Object.getPrototypeOf(nodeList)); // NodeListPrototype

// slice(), includes()는 NodeList 타입에 없음
var nodeList = document.querySelectorAll('div');
console.log(nodeList.includes); // undefined
console.log(nodeList.slice); // undefined

// includes()를 쓰고 싶을 때
var e = nodeList[0];
Array.prototype.includes.call(nodeList, e); // true

// slice()를 쓰고 싶을 때: nodeList를 복제된 배열로 만들기
var arr = Array.prototype.slice.call(nodeList);
console.log(Object.getPrototypeOf(arr)); // Array

// 이제 Array.prototype.map()을 호출 할 수 있음
console.log(arr.map); // function map()
```

## Function.prototype.apply()

```
function.apply( thisArg, [argsArray] )
```

함수를 호출하되 첫 번째 전달인자를 `this`로 설정한다. 두 번째 전달인자는 인자의 개수가 하나여도 반드시 배열이어야 하며, 이 배열은 함수가 호출될 때 함수 내부의 `arguments` 객체로 전달된다. 첫 번째 전달인자는 필수지만 두 번째 전달인자는 생략 가능하다.

```js
function fn() {
  console.debug(this); // Number { 123456 }
  console.debug(arguments[0]); // a
  console.debug(arguments[1]); // b
}
fn.apply(123456, ['a', 'b']);
```

## Function.prototype.call()

```
function.call( thisArg [, arg1 [, arg2 [, ... ] ] ] )
```

`call()`은 `apply()`와 거의 동일하게 작동한다. 유일한 차이점은 추가 인자를 배열이 아닌 쉼표로 구분된 전달인자 목록으로 받는다는 것.

```js
function fn() {
  console.debug(this); // Number { 123456 }
  console.debug(arguments[0]); // a
  console.debug(arguments[1]); // b
}
fn.call(123456, 'a', 'b'); // apply()와는 전달인자의 형태만 다르다.
```

## 엄격 모드에서의 차이

엄격 모드(strict mode)에서는 첫 번째 전달인자가 원시 타입일 때 래퍼 객체로 변환하는 표준 모드와 다르게 원시 타입 그대로 전달한다.

```js
function standard() {
  console.debug(this); // Boolean { false }, 이 값은 원시값을 감싼 래퍼 객체다.
}
standard.call(false);

function strict() {
  'use strict';
  console.debug(this); // true
}
strict.call(true);
```

## 활용: 배열 전개

원래 `Math.min()`의 인자는 배열을 넘기는 것이 불가능하다.

```js
Math.min(3, 2, 4); // 2
Math.min([3, 2, 4]); // NaN
```

`apply()`는 추가 인자를 배열로 받는다는 것을 이용하면 아래처럼 가능함:

```js
Math.min.apply(Math, [3, 2, 4]); // 2
// Math.min.apply(null, [3, 2, 4]);
```

첫 번째 인자로 null을 전달해도 결과는 같은데, 애초에 `Math.min()`이 스태틱 메서드라 가능한 것.

## 꼐속
