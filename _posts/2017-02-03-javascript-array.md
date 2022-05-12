---
layout: post
date: 2017-02-03 13:44:00 +0900
title: '[JavaScript] Array'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - array
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm](http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm)
- [Array 객체에서 놓치기 쉬운 6개의 메서드](http://programmingsummaries.tistory.com/357)

## 개요

Array 생성자 함수와 Array.prototype의 주요 프로퍼티와 메서드를 정리한 글.

## 스태틱 프로퍼티

TODO

## 스태틱 메서드

### [Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

```
Array.from( arrayLike, function mapFn( element, index ) { /* ... */ } [, thisArg] )
```

array-like이거나 반복 가능한 객체(iterable object)를 인자로 받아 얕게 복제된 배열을 반환한다.

```js
Array.from('abc'); // Array(3) [ "a", "b", "c" ]

var nodeList = document.querySelectorAll('div');
Array.from(nodeList) // Array(32) [ div, div, ... ]
```

## 인스턴스 프로퍼티

`length` 하나 있지 뭐.

TODO

## 인스턴스 메서드

### Array.prototype.push()

배열의 맨 뒤에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.push('a'); // 1
arr.push('b', 'c', 'd'); // 4
arr; // ['a', 'b', 'c', 'd']
```

### Array.prototype.unshift()

`push()`와 반대로 배열의 맨 앞에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.unshift('a'); // 1
arr.unshift('b', 'c', 'd'); // 4
arr; // ['b', 'c', 'd', 'a']
```

### Array.prototype.pop()

맨 뒤 요소를 뽑음(?)

### Array.prototype.shift()

맨 앞 요소를 뽑음

### [Array.prototype.includes()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

있는지 확인(마치 `contains()`처럼).

```js
let arr = ['a', 'b', 'c'];
arr.includes('b'); // true
```

### [Array.prototype.map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

```
arr.map( function callback( element, index, array ) [, thisArg] )
```

- `thisArg`: optional이고 "Value to use as this when executing callback function." 라는데 뭔 소리람. 🤔

주어진 함수가 반환하는 결과로 새로운 배열을 생성해 반환. 이건 왜 이름을 이렇게 지었는지 의문이다.

```js
[ 1, 2, 3, 4, 5 ].map(n => n * 2); // [ 2, 4, 6, 8, 10 ]

[ 1, 2, 3, 4, 5 ].map(function(n) {
  return n == 1;
});
// [ true, false, false, false, false ]
```

### Array.prototype.forEach()

```
arr.forEach( function callback( element, index, object ) [, thisArg ] )
```

TODO: 자바에서 `forEach()`류(정확히는 stream이지만)에선 continue/break 사용이 불가능했는데, js도 그러는지 확인 필요함.

### Array.prototype.filter()

주어진 조건을 만족하는 새 배열을 반환한다. `true` 혹은 `false`를 반환하는 함수를 인자로 받는다.

```js
var arr = [1, 2, 3, 4];
var even = arr.filter(function(element) {
  return element % 2 == 0;
});
even; // [2, 4]
arr === even; // false
```

### Array.prototype.some()

### Array.prototype.every()

### Array.prototype.reduce()

### [Array.prototype.reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

배열의 순서를 거꾸로 뒤집은 배열을 반환한다. **원본이 변화한다.**

```js
var arr = ['a', 'b', 'c'];
arr.reverse(); // Array(3) [ "c", "b", "a" ]
arr; // Array(3) [ "c", "b", "a" ]
```

### [Array.prototype.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

특정 인덱스 범위의 요소를 잘라내 반환하는 메서드. 원본은 변하지 않는다.

```
arr.slice( [ begin [, end ] ] )
```

음수를 지정하면 배열의 끝에서부터의 길이를 의미한다. `end`를 생략하면 `begin`부터 끝까지 잘라낸다.

`begin`과 `end` 둘 다 생략하면 기존 배열의 얕은 복제본을 반환한다.

```js
var arr = ['가', '나', '다', '라', '마'];

// 배열 복제
var newArr = arr.slice(); // Array(5) [ "가", "나", "다", "라", "마" ]
console.log(newArr === arr); // false, 내용물은 같지만 서로 다른 인스턴스

// 세 번째 자리부터 끝까지
arr.slice(2); // Array(3) [ "다", "라", "마" ]

// 끝에서 (역순으로) 두 번째 자리까지
arr.slice(-2) // Array [ "라", "마" ]

// 두 번째 자리부터 다섯 번째 자리 전까지
arr.slice(1, 4) // Array(3) [ "나", "다", "라" ]

// 두 번째 자리부터 끝까지 반환하되, 뒤에서 두 개는 뺌
arr.slice(1, -2) // Array [ "나", "다" ]

// 세 번째 자리부터 끝까지 반환하되, 뒤에서 하나는 뺌
arr.slice(1, -1) // Array [ "다", "라" ]
```

### Array.prototype.splice()

```
arr.splice( start [, deleteCount [, item1 [, item2 [, ...] ] ] ] )
```

### [Array.prototype.fill()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

배열의 특정 인덱스를 주어진 값 하나로 채우거나 뒤바꾸는 메서드.

```
arr.fill( value [, start [, end] ] )
```

```js
// 6개짜리 배열을 모두 null로 채움
var arr = Array(6).fill(null);
// Array(6) [ null, null, null, null, null, null ]

// 숫자 0으로 세 번째 자리부터 다섯 번째 자리 전까지 채
arr.fill(0, 2, 4);
// Array(6) [ null, null, 0, 0, null, null ]

// 숫자 5로 두 번째 자리부터 끝까지 채움
arr.fill(5, 1);
// Array(6) [ null, 5, 5, 5, 5, 5 ]
```

### Array.prototype.join()

### [Array.prototype.concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

주어진 배열이나 값을 이어붙인 새 배열을 반환한다. 원본은 변하지 않는다.

```
arr.concat( [value1 [, value2 [, ... [, valueN] ] ] ] )
```

인자를 생략하면 기존 배열의 얕은 복제본(shallow clone)을 반환한다.

```js
const alphabet = ['a', 'b', 'c'];
const numeric = [1, 2, 3];

// 배열 두 개 이어붙이기
alphabet.concat(numeric); // Array(6) [ "a", "b", "c", 1, 2, 3 ]

// 배열 세 개 이어붙이기
alphabet.concat(numeric, numeric); // Array(9) [ "a", "b", "c", 1, 2, 3, 1, 2, 3 ]

// 기존 배열에 요소 추가
alphabet.concat(['d', 'e']) // Array(5) [ "a", "b", "c", "d", "e" ]

// 기존 배열에 요소 추가 #2
numeric.concat(4, 5, 6); // Array(6) [ 1, 2, 3, 4, 5, 6 ]

const objArr = [
  { a: 1 },
  { b: 2 }
];
objArr; // Array [ { a: 1 }, { b: 2 } ]

// 객체 배열에 새 객체 요소 추가
objArr.concat([{ c: 3 }]); // Array(3) [ { a: 1 }, { b: 2 }, { c: 3 } ]
```
