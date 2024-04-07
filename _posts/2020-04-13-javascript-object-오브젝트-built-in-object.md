---
layout: post
date: 2020-04-13 09:49:00 +0900
title: '[JavaScript] Object'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - Object
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN \| Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [MDN \| Object prototypes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)


## 개요

`Object`와 `Object.prototype` 차이, 주요 메서드 등을 정리함.

**TODO** 생성자 함수는 스태틱, 프로토타입은 인스턴스로 변경할 것


## Object vs Object.prototype

`Object`는 `Object.prototype`의 생성자 함수이면서 동시에 객체 관련 유틸성 메서드를 제공하는 표준 내장 객체다. 프로토타입이 아니므로 `Object`의 프로퍼티와 메서드는 상속되지 않는다.

`Object.prototype`은 모든 객체의 조상, 최상위 프로토타입인 `Object` 프로토타입이다.

`Object`의 메서드는 `Object.fn()` 같은 형태로 호출하니 스태틱 메서드, `Object.prototype`의 메서드는 인스턴스로 만들어진 객체를 통해 호출하니 인스턴스 메서드로 분류한다.


## 스태틱 메서드

### Object.getOwnPropertyDescriptor(), Object.getOwnPropertyDescriptors()

객체의 모든 프로퍼티 설명자 혹은 특정 프로퍼티의 설명자를 반환한다. 설명자란 해당 프로퍼티가 어떻게 설정(쓰기 가능한지, 열거 가능한지 등등)되어 있는지를 알려주는 객체다.

객체 자신이 소유한 프로퍼티가 아니면 반환되지 않는다. 가령 새로 정의한 객체`{a: 1}`는 소유한 프로퍼티가 `a`만 있으므로 `toString`의 프로퍼티 설명자는 안나온다.

```js
var o = {a: 1};
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

### Object.getOwnPropertyNames()

특정 객체가 소유한 프로퍼티들의 이름을 배열로 반환한다.

```js
// String 프로토타입이 소유한 프로퍼티 중 이름에 'sub'가 포함된 것만 필터링
Object.getOwnPropertyNames(String.prototype).filter((e) => {
  return e.toLowerCase().indexOf('case') > -1;
});
// Array(4) [ "toLowerCase", "toUpperCase", "toLocaleLowerCase", "toLocaleUpperCase" ]
```

전달인자가 프로토타입인지 혹은 인스턴스인지에 따라서 같은 타입이라도 결과가 다르다. 메서드의 소유자는 프로토타입이지만, 필드의 소유자는 인스턴스이기 때문:

```js
class Newbie {
  msg = 'Me iz da best!';
  #secret = 'Something special...';

  constructor(trait) {
    this._trait = trait;
  }

  levelUp() {
    console.log('I feel stronger.');
    this._trait = 'Can barely shoot an arrow.';
  }
}

var noob = new Newbie();

Object.getOwnPropertyNames(noob); // Array [ "msg", "_trait" ]
Object.getOwnPropertyNames(Newbie.prototype); // Array [ "constructor", "levelUp" ]
```

단, 전달인자가 인스턴스여도 private 필드는 반환되지 않는다.


### Object.defineProperty()

객체에 새로운 프로퍼티를 정의하거나 이미 존재하는 프로퍼티를 수정하는 메서드.

```js
var obj = {};
Object.defineProperty(obj, 'name', {
  value: 'waldo', 
  writable: false, 
  enumerable: true
});

Object.getOwnPropertyDescriptor(obj, 'name');
// Object { value: "waldo", writable: false, enumerable: true, configurable: false }
```

### Object.create()

```
Object.create(proto)
Object.create(proto, propertiesObject)
```

주어진 객체로 새 인스턴스를 만들어 반환한다. 이 때 `proto`는 인스턴스의 프로토타입이 된다.

`proto`가 `null`이면 자바스크립트의 순수 사전식(pure dictionary) 객체인 null 프로토타입 객체(null-prototype objects)가 만들어진다.

```js
var plainObject = Object.create(null);
Object.getPrototypeOf(plainObject); // null
```

null 프로토타입은 프로토타입이 `null`이란 뜻이며, 상속받을 프로퍼티나 메서드가 아무것도 없어서 일반적인 객체처럼 다루면 예상치 못한 오류가 발생할 수 있다:

```js
console.log(`${plainObject}`); // Uncaught TypeError: can't convert plainObject to string
```

### Object.assign()

```
Object.assign(target, source1)
Object.assign(target, source1, source2, ...)
```

주어진 객체 고유의 열거 가능한 모든 프로퍼티를 `target`에 복사한다. `target`의 원본이 변화(mutating)하며, 변화된 `target`을 반환한다.

```js
var o1 = {a: 1, b: 2};
var o2 = {c: 3};
var o3 = Object.assign(o1, o2);

console.log(o1); // Object { a: 1, b: 2, c: 3 }
console.log(o3); // Object { a: 1, b: 2, c: 3 }
console.log(o1 === o3); // true
```

### Object.keys

주어진 객체 고유의 열거 가능한 모든 프로퍼티의 이름을 배열로 반환한다.

```js
var loopMe = {
  a: 7,
  b: 8,
  c: 9
};
Object.defineProperty(loopMe, 'd', {value: 10, enumerable: false});

Object.keys(loopMe); // Array(3) [ "a", "b", "c" ]
```

### Object.values

주어진 객체 고유의 열거 가능한 모든 프로퍼티의 값을 배열로 반환한다.

```js
var obj = {
  str: 'abc',
  num: 123,
  boo: true
};
Object.defineProperty(obj, 'bar', {value: 10, enumerable: false});

Object.values(obj); // Array(3) [ "abc", 123, true ]
```

### Object.entries

주어진 객체 고유의 열거 가능한 모든 프로퍼티의 값과 이름 쌍(pair)을 배열로 반환한다.

```js
var loopMe = {
  a: 7,
  b: 8,
  c: 9
};

Object.entries(loopMe); // Array(3) [ [ "a", 7 ], [ "b", 8 ], [ "c", 9 ] ]
```

### Object.freeze()

주어진 객체의 변형과 확장을 막는 스태틱 메서드. 

```
Object.freeze(obj)
```

- `obj`: `obj`의 프로퍼티들은 쓰기 불가(non-writable), 설정 불가(non-configurable) 상태가 된다.

이 메서드는 파라미터로 주어진 객체를 얼려서🥶 반환한다.

```js
var beer = { temperature: -1 };
var o = Object.freeze(beer);
console.log(beer === o); // true
```

한 번 얼은 객체는 프로퍼티를 추가하거나 재할당 할 수 없다:

```js
var icecream = Object.freeze({a: 1, b: 2});
console.log(icecream); // {a: 1, b: 2}
icecream.c = 3
console.log(icecream); // {a: 1, b: 2}
```

그리고 내부의 중첩된 객체까지 얼리진 못하기 때문에, 객체 리터럴마다 적용해야 함:

```js
// 중첩된 객체의 변화는 막을 수 없음
var freezeMe1 = Object.freeze({
    a: 1, 
    b: 2, 
    c: { d: 4 }
});
console.log(freezeMe1); // Object { a: 1, b: 2, c: { d: 4 } }
freezeMe1.c.d = 567;
console.log(freezeMe1); // Object { a: 1, b: 2, c: { d: 567 } }

// 중첩 객체에 별도로 적용하면 막을 수 있다
var freezeMe2 = Object.freeze({
    a: 1, 
    b: 2, 
    c: Object.freeze({ d: 4 })
});
console.log(freezeMe2); // Object { a: 1, b: 2, c: { d: 4 } }
freezeMe2.c.d = 567
console.log(freezeMe2); // Object { a: 1, b: 2, c: { d: 4 } }
```

한 번 얼린 객체를 되돌리는 방법은 없다. 멀쩡한 게 필요하면 복제를 하면 되는데, 중첩 객체가 있을 때는 깊게 복제해야 함.

### Object.is()

주어진 값 두 개가 [같은 값(same value)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness#same-value_equality_using_object.is)한지 판단한다.

```
Object.is(value1, value2)
```

`Object.is()`는 동등 연산자`==`나, 일치 연산자`===`와 다르게 작동한다. 둘 중 일치 연산자`===`와 비슷하게 작동하긴 한다.

같은 값으로 판단하는 경우는 다음과 같다:

- 둘 다 `undefined`
- 둘 다 `null`
- 둘 다 `true` 혹은 둘 다 `false`
- 둘 다 같은 문자열(정확히는 같은 순서와 같은 케릭터에 같은 길이)
- 둘 다 같은 객체(메모리 상 같은 객체를 가리키는지)
- 둘 다 숫자이며 `+0`
- 둘 다 숫자이며 `-0`
- 둘 다 숫자이며 `NaN`
- 둘 다 숫자이며 `0`이나 `NaN`이 아닌 같은 값

```js
Object.is(undefined, undefined); // true
Object.is(null, null); // true
Object.is(undefined, null); // false

Object.is(true, true); // true
Object.is(false, false); // true
Object.is(true, false); // false

Object.is('abc', 'abc'); // true
Object.is('bca', 'abc'); // false
Object.is('abcd', 'abc'); // false

Object.is(0, 0); // true
Object.is(-0, -0); // true
Object.is(0, -0); // false

Object.is(NaN, NaN); // true
Object.is(NaN, 256); // false

Object.is(256, 256); // true
Object.is(256, 128); // false

var obj = {};
Object.is(obj, obj); // true
Object.is(obj, {}); // false
```


## 인스턴스 메서드

### Object.prototype.hasOwnProperty()

**TODO**

어떤 프로퍼티가 `obj`의 자체 프로퍼티(own property, 상속받은 게 아니라 객체가 소유한 프로퍼티)인지를 불리언 값으로 반환한다.


### Object.prototype.propertyIsEnumerable()

```
object.propertyIsEnumerable(propertyName)
```

`propertyName`와 같은 이름의 프로퍼티가 `obj`의 열거 가능한 자체 프로퍼티(enumerable own property)로 존재하는지를 불리언 값으로 반환한다.

```js
var str = 'foo';
str.propertyIsEnumerable('toString'); // false

var obj = {
  a: 'bar'
};
obj.propertyIsEnumerable('a'); // true
```


## 꼐속...
