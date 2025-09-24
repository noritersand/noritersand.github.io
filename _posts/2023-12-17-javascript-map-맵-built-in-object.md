---
layout: post
date: 2023-12-17 15:26:10 +0900
title: '[JavaScript] Map'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - map
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Map \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)


## 개요

표준 내장 객체 `Map`에 대한 정리 글.

`Map`은 키-값(key-value) 쌍을 저장하는 데이터 구조다. 자바스크립트에서 Map의 키는 문자열 외에도 `Symbol`이나 숫자같은 데이터 타입도 허용된다. 

키는 유일성이 보장되며, 동일한(= 중복되는) 키를 반복 사용하면 기존의 값을 덮어씌우게 된다. 그리고 `Map`은 요소가 추가된 순서를 기억한다.

```js
var m = new Map();
m.set(123, '456');
m.get(123); // "456"

m.size; // 1
m.delete(123); // true
m.size; // 0

var m2 = new Map();
m2.set('b', 1);
m2.set('c', 1);
m2.set('a', 1);

log(m2.keys()); // MapIterator { "b", "c", "a", }
```

## 인스턴스 프로퍼티

Map에 저장된 데이터의 수를 반환하는 `size` 하나가 끝.


## 스태틱 메서드

### Map.groupBy()

```
Map.groupBy(items, callbackFn)

function callbackFn(element, index) {}
```

`Map`을 이용한 그룹화 함수. 배열과 사용자 정의 함수를 인자로 받으며, 배열의 구성 요소를 사용자 정의 함수에서 반환한 값으로 묶어 `Map` 타입으로 반환한다:

```js
var inventory = [
  { name: 'asparagus', quantity: 9 },
  { name: 'bananas', quantity: 5 },
  { name: 'goat', quantity: 23 },
  { name: 'cherries', quantity: 12 },
  { name: 'fish', quantity: 22 },
];

var result = Map.groupBy(inventory, element => {
  return element.quantity > 9 ? 'greaterThan' : 'lesserThanOrEqualTo'
});

console.log(result instanceof Map); // true;

console.log(result.get('greaterThan'));
// [{ name: "goat", quantity: 23 }, { name: "cherries", quantity: 12 }, { name: "fish", quantity: 22 }]

console.log(result.get('lesserThanOrEqualTo'));
// [{ name: "asparagus", quantity: 9 }, { name: "bananas", quantity: 5 }]
```

최신(2024) 기술이라 아직 지원하지 않는 브라우저가 있음.


## 인스턴스 메서드

### Map.prototype.clear()

인스턴스의 모든 요소를 제거한다.

```js
var m = new Map();
m.set('a', 1);
m.set('b', 2);

console.log(m.size); // 2
m.clear();
console.log(m.size); // 0
```

### Map.prototype.delete()

인스턴스에서 특정 요소를 제거한다. 첫 번째 인자로 키 이름을 받는다.

```js
var m = new Map();
m.set('a', 1);
m.set('b', 2);

m.delete('b');
console.log(m); // Map(1) { "a": 1, }
```

### Map.prototype.get()

평범한 getter니까 생략

### Map.prototype.set()

setter도 생략

### Map.prototype.has()

특정 키의 요소가 있는지를 `boolean` 타입으로 반환한다.

```js
var map = new Map();
console.log(map.has('foo')); // false
```

### Map.prototype.entries()

`Map` 인스턴스에 추가된 순서 그대로의 키-값 쌍을 갖는 이터레이터(iterator)를, 호출할 때마다 새로 만들어 반환한다.

```js
var m = new Map();
m.set('a', 1);
m.set('b', 2);

console.log(m.entries()); // Map Iterator { [ "a", 1 ], [ "b", 2 ] }
```

### Map.prototype.keys()

`entries()`와 비슷한데 이 메서드가 반환하는 이터레이터는 키만 갖는다.

```js
console.log(m.keys()); // Map Iterator { "a", "b" }
```

### Map.prototype.values()

이 메서드는 이터레이터가 키 대신 값만 갖는다.

```js
console.log(m.values()); // Map Iterator { 1, 2 }
```

### Map.prototype.forEach()

```
map.forEach(callbackFn)
map.forEach(callbackFn, thisArg)

function callbackFn(value, key, map)
```

인스턴스의 키-값 쌍이 추가된 순서대로 반복하며 사용자 정의 함수를 실행한다. 

```js
var m = new Map();
m.set('a', 1);
m.set('b', 2);

m.forEach((value, key, map) => {
  console.log('value:', value);
  console.log('key:', key);
  console.log('map:', map);
});

// value: 1
// key: a
// map: Map(2) { "a": 1, "b": 2, }

// value: 2
// key: b
// map: Map(2) { "a": 1, "b": 2, }
```

같은 이름인 `Array.prototype.forEach()`와 비교하면 콜백 함수 매개변수에서 차이가 있다(`Array`는 `(element, index, object)`).


끟.
