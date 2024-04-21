---
layout: post
date: 2016-07-22 19:00:10 +0900
title: '[JavaScript] Function'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - function
  - apply
  - call
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Function \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
- [Function.prototype.apply() \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
- [Function.prototype.call() \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
- [Function.prototype.bind() \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
- [https://stackoverflow.com/questions/38736902/settimeout-bind-and-this](https://stackoverflow.com/questions/38736902/settimeout-bind-and-this)


## 개요

표준 내장 객체 `Function`의 프로퍼티와 메서드를 정리한 글.


## 인스턴스 메서드

인스턴스 메서드는 상속되기 때문에 `Function` 객체, 즉 모든 함수는 `apply()`, `call()`, `bind()`를 호출할 수 있다. 이 메서드들은 함수를 특별하게 호출하기 위해 사용한다.

예를 들어 `NodeList` 타입은 반복이 가능한 유사배열(array-like) 타입이지만 `Array` 프로토타입의 파생형이 아니다. 그래서 `Array.prototype.every()`나 `Array.prototype.slice()` 같은 메서드를 사용할 수 없는 문제가 있는데 이 때 `apply()`나 `call()`을 쓴다:

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

### Function.prototype.apply()

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

#### 활용 방법: 배열 전개

원래 `Math.min()`의 인자는 배열을 넘기는 것이 불가능하다.

```js
Math.min(3, 2, 4); // 2
Math.min([3, 2, 4]); // NaN
```

사실 이럴 땐 전개하면 끝이지만...

```js
Math.min(...[3, 2, 4]); // 2
```

`apply()`가 추가 인자를 배열로 받는다는 것을 이용하면 아래처럼도 가능하다:

```js
Math.min.apply(Math, [3, 2, 4]); // 2
Math.min.apply(null, [3, 2, 4]); // 2
```

첫 번째 인자로 `null`을 전달해도 결과는 같은데, 어차피 `Math.min()`은 스태틱 메서드고 `this`를 참조하지 않기 때문에 뭐가 됐든 상관 없어서 그렇다.

### Function.prototype.call()

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

#### 엄격 모드에서의 차이

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

### Function.prototype.bind()

```
function.bind( thisArg [, arg1 [, arg2 [, ... ] ] ] )
```

함수의 `this`를 교체하고 고정 인수를 지정하는 메서드. 이 메서드는 호출될 때 제공된 첫 번째 전달인자를 `this` 키워드로 지정하고, 두 번째 전달인자부터는 함수의 고정 인수로 지정한 새 함수를 생성하여 반환한다. 이 작업을 바인딩이라고 표현하는 모양이다.

ES2015에서 정의된 Exotic function object에 속한다고 한다. 다른 Exotic function object로 `Array`, `String`, `Arguments`, `Proxy` 등이 있는데, 특수한 기능을 수행하는 객체라고 하지만 정확하게 뭘 하는지를 아직 몲. 

#### this의 교체

```js
function unboundFn() {
  return this.a;
}
unboundFn(); // undefined

var replacement = {
  a: 5
}

var boundFn = unboundFn.bind(replacement); // this를 obj로 교체
boundFn(); // 5
```

원래 `this`는 함수를 어떻게 호출하느냐에 따라 달라진다. (엄격 모드가 아닐 때) 함수로 호출하면 전역객체가, 메서드로 호출하면 메서드를 소유한 객체가 `this`인데, 바인딩을 하면 이를 무시한다:

```js
var obj4 = {
  x: 123,
  fn() {
    return this.x;
  }
};

obj4.fn(); // 123

var unboundFn4 = obj4.fn;
unboundFn4(); // undefined

// 함수의 this를 obj로 바인딩하면 함수로 호출해도 123이 반환됨
var boundFn4 = unboundFn4.bind(obj4);
boundFn4(); // 123
```

만약 엄격 모드가 아닐 때 `bind()`의 첫 번째 전달인자가 `null` 혹은 `undefined`면, `this`는 전역 객체로 대체된다:

```js
function unboundFn2() {
  return this;
}
unboundFn2(); // Window

var boundFn2 = unboundFn2.bind(null);
boundFn2(); // Window

// 반면 엄격 모드에선
function strictFn2() {
  'use strict';
  return this;
}
strictFn2(); // undefined

var boundStrictFn2 = strictFn2.bind(null);
boundStrictFn2(); // null
```

#### 고정 인수 설정

두 번째 전달인자부턴 새 함수의 고정 인수가 된다:

```js
function unboundFn3(...args) {
  console.log('args:', args);
  console.log('arguments:', arguments);
}
unboundFn3();
// args: Array []
// arguments: Arguments { … }

var boundFn3 = unboundFn3.bind(null, 1, 2);

boundFn3();
// args: Array [ 1, 2 ]
// arguments: Arguments { 0: 1, 1: 2, … }

boundFn3(3, 4);
// args: Array(4) [ 1, 2, 3, 4 ]
// arguments: Arguments { 0: 1, 1: 2, 2: 3, 3: 4, … }

function unboundFn4(a, b) {
  console.log('a:', a);
  console.log('b:', b);
}
unboundFn4();
// a: undefined
// b: undefined

var boundFn4 = unboundFn4.bind(null, 1);

boundFn4();
// a: 1
// b: undefined

boundFn4(2);
// a: 1
// b: 2
```
