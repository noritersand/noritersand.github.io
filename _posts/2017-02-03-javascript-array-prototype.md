---
layout: post
date: 2017-02-03 13:44:00 +0900
title: '[JavaScript] Array prototype'
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

#### 관련 문서

- [\[MDN\] Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm](http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm)
- [Array 객체에서 놓치기 쉬운 6개의 메서드](http://programmingsummaries.tistory.com/357)


## 개요

`Array.prototype`의 주요 프로퍼티, 메서드를 정리한 글.


## 스태틱 프로퍼티

TODO


## 스태틱 메서드

### Array.of()

TODO

`Array` 인스턴스를 반환한다.

### Array.from()

```
Array.from( arrayLike, function mapFn( element, index ) { /* ... */ } [, thisArg] )
```

유사 배열(array-like)이거나 반복 가능한 객체(iterable object)를 인자로 받아 얕게 복제된 배열을 반환한다.

```js
Array.from('abc'); // Array(3) [ "a", "b", "c" ]

var nodeList = document.querySelectorAll('div');
Array.from(nodeList) // Array(32) [ div, div, ... ]
```


## 인스턴스 프로퍼티

`length` 하나 있지 뭐

TODO


## 인스턴스 메서드

### Array.prototype.at()

```
array.at(index)
```

지정한 인덱스의 요소를 반환한다. 대괄호 표현식과 유일한 다른 점은 인덱스를 음수로 지정할 수 있다는 것인데, 인덱스가 음수면 순서를 거꾸로 계산한다:

```js
var arr = ['a', 'b', 'c'];
arr.at(0); // "a"
arr.at(1); // "b"
arr.at(2); // "c"
arr.at(3); // undefined
arr.at(-1); // "c"
arr.at(-2); // "b"
arr.at(-3); // "a"
arr.at(-4); // undefined 
```

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

맨 뒤 요소를 반환하고 **원본 배열에서 제거**한다.

```js
var arr = ['a', 'b', 'c'];
arr.pop(); // "c"
arr; // Array [ "a", "b" ]
````

### Array.prototype.shift()

맨 앞 요소를 반환하고 **원본 배열에서 제거**한다.

```js
var arr = ['a', 'b', 'c'];
arr.shift(); // "a"
arr; // Array [ "b", "c" ]
````

### Array.prototype.indexOf()

TODO

### Array.prototype.lastIndexOf()

TODO

### Array.prototype.includes()

있는지 확인한다. `contains()`와 비슷함.

```js
let arr = ['a', 'b', 'c'];
arr.includes('b'); // true
```

### Array.prototype.find()

```
find( function( element, index, array ) { /* ... */ }[, thisArg])
```

- `thisArg`: 콜백함수에서 `this`로 사용할 객체를 지정한다. 추가 내용은 [여기](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods)를 볼 것

콜백 함수에 요소를 전달하며 배열의 길이만큼 반복 실행한다. 콜백 함수에서 `true`를 반환하면 반복을 중단하며, 해당 시점의 배열 요소를 최종 반환한다. 만약 모든 루프에서 `true`를 반환하지 않으면 최종 반환값은 `undefined`다.

```js
let arr = ['a', 'b', 'c'];
arr.find(ele => {
  return ele == 'b'; // 이 조건 때문에 'c'는 생략됨
});
// 'b'

let arr2 = ['a', 'b', 'c'];
arr.find(ele => {
  return ele == 'e';
});
// undefined
```

\* `find()`, `map()`, `filter()` 처럼 함수를 인자로 받거나 반환하면 고차 함수(higher-order functions)라고 한다.

### Array.prototype.findIndex()

```
findIndex(function(element, index, array) { /* ... */ }[, thisArg])
```

`find()`와 같으나 반환하는 값이 배열 요소의 인덱스라는 점만 다르다. 만약 모든 루프에서 `true`를 반환하지 않으면 최종 반환값은 `-1`이다.

```js
let arr3 = ['a', 'b', 'c'];
arr.findIndex(ele => {
  return ele == 'b'; // 'c'는 생략됨
})
// 1 

let arr4 = ['a', 'b', 'c'];
arr.findIndex(ele => {
  return ele == 'e';
});
// -1
```

### Array.prototype.map()

```
arr.map( function callback( element, index, array ) [, thisArg] )
```

- `thisArg`: optional이고 "Value to use as this when executing callback function." 라는데 뭔 소리람. 🤔

주어진 함수가 반환하는 결과로 새로운 배열을 생성해 반환. 왜 이름을 이렇게 지었는지 의문이다.

```js
[1, 2, 3, 4, 5].map(n => n * 2); // [ 2, 4, 6, 8, 10 ]

[1, 2, 3, 4, 5].map(function(n) {
  return n == 1;
});
// [ true, false, false, false, false ]
```

### Array.prototype.forEach()

```
arr.forEach( function callback( element, index, object ) [, thisArg ] )
```

`forEach()`는 `continue`와 `break` 사용이 불가능한데, `continue`는 `return`으로 구현할 수 있다. 하지만 `break`는 방법이 없으니(`return false`를 반환해 멈추는 건 jQuery에서나 가능했던 방법), 필요한 경우엔 for statement를 쓰도록 하자.

### Array.prototype.filter()

```
filter(function(element, index, array) { /* ... */ }, thisArg)
```

- `element`: 배열의 각 요소
- `index`: 인덱스
- `array`: 배열 전체

특정 조건으로 필터링한 새 배열을 반환한다. 콜백 함수에서 `true` 혹은 `false`를 반환해야 하며, `true`가 아닐 경우 해당 요소를 제외한 배열을 반환한다. 원본은 변하지 않는다.

```js
var arr = [1, 2, 3, 4];
var even = arr.filter(function(element) {
  return element % 2 == 0;
});
even; // [2, 4]
arr === even; // false
```

```js
// 중복 요소만 필터링해서 반환
array.filter((ele, idx, arr) => {
  return arr.indexOf(ele) !== arr.lastIndexOf(ele)
});
```

### Array.prototype.every()

```
every(function(element, index, array) { /* ... */ }, thisArg)
```

`forEach()`와 같이 배열의 모든 요소를 콜백함수 첫 번째 파라미터로 제공한다. 다만 모든 루프에서 `return true`면 `true`, 하나라도 `return false`가 되면 `false`를 반환하는 특징을 이용해서 모든 요소가 특정 테스트를 통과하는지 확인할 때 사용한다.

```js
[1, 2, 3].every(ele => isFinite(ele)); // true
```


가령 다음의 경우:

```js
const obj = {
  a: null,
  b: null
};
const isNullish = Object.values(obj).every(value => {
  if (value === null) {
    return true;
  }
  return false;
});
console.log(isNullish); // true
// 코드 출처: https://bobbyhadz.com/blog/javascript-check-if-all-object-properties-are-null
```

`obj`의 프로퍼티 `a`와 `b`의 값이 모두 `null`이므로 모든 요소가 `return true`가 되고 `every()`는 결국 `true`를 반환하게 되는 코드다.

그리고 `return false`가 되는 시점에 반복을 중단하는 특징이 있다:

```js
[1, 2, 3].every(ele => {
  console.log(ele);
  return ele !== 1;
});
// 1
```

### Array.prototype.some()

```
some(function(element, index, array) { /* ... */ }, thisArg)
```

`every()`와 비슷하지만 반대로 작동한다. 딱 하나의 루프만이라도 `return true`면 `true`를 반환한다:

```js
[1, 2, 3].some(ele => ele === 1); // true
```

그리고 한 번 `return true`면 그대로 반복을 중단한다:

```js
[1, 2, 3].some(ele => {
  console.log(ele);
  return ele === 1;
}); 
// 1

[1, 2, 3].some(ele => {
  console.log(ele);
  return ele === 3;
}); 
// 1
// 2
// 3
```

### Array.prototype.reduce()

```
arr.reduce(function(previousValue, currentValue, currentIndex, array) { /* ... */ }, initialValue)
```

전달한 콜백함수에 인자로 이전 요소의 연산결과를 넘겨준다. 숫자 배열의 합계 같은 모든 요소에 대한 처리 결과값이 필요할 때 사용한다. 왠지 뭔가 감소할 것 같은 메서드 이름과 다르게, 원본 배열은 변하지 않는다.

```js
var arr = [1, 2, 3, 4];
arr.reduce((prev, cur) => prev + cur);  // 10
```

### Array.prototype.reverse()

배열의 순서를 거꾸로 뒤집은 배열을 반환한다. **원본이 변한다.**

```js
var arr = ['a', 'b', 'c'];
arr.reverse(); // Array(3) [ "c", "b", "a" ]
arr; // Array(3) [ "c", "b", "a" ]
```

### Array.prototype.slice()

```
arr.slice( [ begin [, end ] ] )
```

특정 인덱스 범위의 요소를 잘라내 반환하는 메서드. 원본은 변하지 않는다.

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

지정한 인덱스인 `start`부터 `deleteCount`만큼 잘라내 반환한다. 잘라내어진 요소는 **원본 배열에서 사라진다**.

```js
var arr = ['a', 'b', 'c'];
arr.splice(0); //Array(3) [ "a", "b", "c" ]
arr; // Array []

var arr = ['a', 'b', 'c'];
arr.splice(1, 2); // Array [ "b", "c" ]
arr; // Array [ "a" ]
```

세 번째 인자 부터는 원본 배열에 이어 붙일 값을 지정할 수 있다:

```js
var arr = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];

// arr의 두 번째 요소부터 두 개를 잘라낸 다음 arr2와 'g'를 이어 붙임
console.log(arr.splice(1, 2, ...arr2, 'g')); // Array [ "b", "c" ]
console.log(arr); // Array(5) [ "a", "d", "e", "f", "g" ]
```

추가 인자가 있어도 `splice()` 반환값이 '잘라내어진 요소들'이란 것은 여전하다.

### Array.prototype.fill()

```
arr.fill( value [, start [, end] ] )
```

배열의 특정 인덱스를 주어진 값 하나로 채우거나 뒤바꾸는 메서드.

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

TODO

### Array.prototype.concat()

```
array.concat( [value1 [, value2 [, ... [, valueN] ] ] ] )
```

주어진 배열이나 값을 이어붙인 새 배열을 반환한다. 원본은 변하지 않는다.

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
  {a: 1},
  {b: 2}
];
objArr; // Array [ { a: 1 }, { b: 2 } ]

// 객체 배열에 새 객체 요소 추가
objArr.concat([{c: 3}]); // Array(3) [ { a: 1 }, { b: 2 }, { c: 3 } ]
```

### Array.prototype.sort()

```
array.sort( [function compareFn(a, b) {}] )
```

그냥 정렬. `compareFn`을 생략하면 오름차순으로 알아서 해주는데, 놀랍게도 다중배열에도 쓸 수 있다:

```js
var arr1 = ['a', 'b', 'd', 'c'];
console.log(JSON.stringify(arr1.sort())); // ["a","b","c","d"]

var arr2 = [['a'], ['b'], ['d'], ['c']];
console.log(JSON.stringify(arr2.sort())); // [["a"],["b"],["c"],["d"]]

var arr3 = [[['a']], [['b']], [['d']], [['c']]];
console.log(JSON.stringify(arr3.sort())); // [[["a"]],[["b"]],[["c"]],[["d"]]]

var arr4 = [[[['a']]], [[['b']]], [[['d']]], [[['c']]]];
console.log(JSON.stringify(arr4.sort())); // [[[["a"]]],[[["b"]]],[[["c"]]],[[["d"]]]]

var arr5 = [[[[['a']]]], [[[['b']]]], [[[['d']]]], [[[['c']]]]];
console.log(JSON.stringify(arr5.sort())); // [[[[["a"]]]],[[[["b"]]]],[[[["c"]]]],[[[["d"]]]]] 

// 🙄
```

아마 첫 번째의 첫 번째의 첫 번째의 ... n 번째의 첫 번째 요소를 비교해주는 것 같지만?

```js
var arr6 = [['a', 'f'], ['a', 'c']];
console.log(JSON.stringify(arr6.sort())); // [["a","c"],["a","f"]]

var arr7 = [[0, 4, 5], [0, 4, 9], [0, 4, 1], [0, 4, 0]];
console.log(JSON.stringify(arr7.sort())); // [[0,4,0],[0,4,1],[0,4,5],[0,4,9]]
```

모든 요소를 비교해줌.
