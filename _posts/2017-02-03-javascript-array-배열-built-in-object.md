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
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Array \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm](http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm)
- [Array 객체에서 놓치기 쉬운 6개의 메서드](http://programmingsummaries.tistory.com/357)


## 개요

표준 내장 객체 `Array`의 주요 프로퍼티, 메서드를 정리한 글.


## 스태틱 프로퍼티

`Array[@@species]` 하나 있는데 뭔지 잘 몲.


## 스태틱 메서드

### Array.from()

```
Array.from(arrayLike)
Array.from(arrayLike, mapFn)
Array.from(arrayLike, mapFn, thisArg)

function mapFn(element, index) {}
```

유사 배열(array-like)이거나 반복 가능한 객체(iterable object)를 인자로 받아 얕게 복제된 배열을 반환한다.

```js
Array.from('abc'); // Array(3) [ "a", "b", "c" ]

var nodeList = document.querySelectorAll('div');
Array.from(nodeList) // Array(32) [ div, div, ... ]
```

### Array.fromAsync()

```
Array.fromAsync(arrayLike)
Array.fromAsync(arrayLike, mapFn)
Array.fromAsync(arrayLike, mapFn, thisArg)

function mapFn(element, index) {}
```

반환 값이 Promise 객체라는 점을 빼면 `Array.from()`과 같다.

### Array.isArray()

```js
Array.isArray([]); // true
Array.isArray({}); // false
Array.isArray('[]'); // false
Array.isArray('array'); // false
```

주어진 객체가 `Array` 프로토타입의 인스턴스면 `true`, 아니면 `false` 반환함

### Array.of()

`Array` 인스턴스를 반환한다. 메서드의 인자로 하나 이상의 값을 전달할 수 있는데, 이 값들은 새로 만들어질 배열의 요소가 된다. 인자가 하나도 없으면 빈 배열이 생성된다.

```js
Array.of(); // Array []
Array.of('a', 'b', 'c'); // Array(3) [ "a", "b", "c" ]
```


## 인스턴스 프로퍼티

`length` 하나 있다.


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

### Array.prototype.concat()

주어진 배열이나 값을 이어붙인 새 배열을 반환한다. 원본은 변하지 않는다.

```
array.concat(value1)
array.concat(value1, value2)
array.concat(value1, value2, ...)
```

인자를 생략하면 기존 배열의 얕은 복제본(shallow clone)을 반환한다.

```js
var alphabet = ['a', 'b', 'c'];
var numeric = [1, 2, 3];

// 배열 두 개 이어붙이기
alphabet.concat(numeric); // Array(6) [ "a", "b", "c", 1, 2, 3 ]

// 배열 세 개 이어붙이기
alphabet.concat(numeric, numeric); // Array(9) [ "a", "b", "c", 1, 2, 3, 1, 2, 3 ]

// 기존 배열에 요소 추가
alphabet.concat(['d', 'e']) // Array(5) [ "a", "b", "c", "d", "e" ]

// 기존 배열에 요소 추가 #2
numeric.concat(4, 5, 6); // Array(6) [ 1, 2, 3, 4, 5, 6 ]

var objArr = [
  {a: 1},
  {b: 2}
];
objArr; // Array [ { a: 1 }, { b: 2 } ]

// 객체 배열에 새 객체 요소 추가
objArr.concat([{c: 3}]); // Array(3) [ { a: 1 }, { b: 2 }, { c: 3 } ]
```

### Array.prototype.copyWithin()

배열 내부의 요소를 얕게 복사하여 다른 위치에 얕게 붙여 넣는다. 배열의 전체 길이는 변경되지 않는다. **원본을 변형시키는 메서드다**.

```
array.copyWithin(target, start)
array.copyWithin(target, start, end)
```

- `target`: 복사한 요소를 붙여 넣을 인덱스. (시작 위치)
- `start`: 복사할 시작 인덱스. 생략하면 기본값은 `0`
- `end`: 복사할 끝 인덱스. 생략하면 기본값은 배열의 길이다. 이 인덱스는 복사에 포함되지 않는다(`end`의 앞까지 복사한다).

아직 뭔 소린지 모르겠다. 아래 예시들을 보자:

```js
var array = [1, 2, 3, 4, 5];
array.copyWithin(0, 3); // Array(5) [ 4, 5, 3, 4, 5 ]
```

위 코드는 인덱스 3부터 끝까지인 `4`와 `5`를 복사하여 인덱스 0인 맨 앞에 붙여 넣는다. 이때 기존 요소는 어디론가 가는 게 아니고 덮어 씌워진다.

```js
var array = ['a', 'b', 'c', 'd', 'e'];
array.copyWithin(1, 2, 3); // Array(5) [ "a", "c", "c", "d", "e" ]
```

이건 인덱스 2부터 인덱스 3의 **앞**까지인 `'c'`를 복사하여 인덱스 1인 `'b'` 자리에 붙여 넣는다. 덮어 씌우기이므로 결과적으로 `'b'`가 `'c'`로 교체된 결과가 된다.

```js
var array = ['a', 'b', 'c', 'd', 'e'];
array.copyWithin(1, 2, 4); // Array(5) [ "a", "c", "d", "d", "e" ]
```

이번에는 인덱스 2부터 인덱스 4의 앞까지인 `'c', 'd'`를 복사하여 `'b', 'c'` 자리에 붙여 넣기다. 

매개변수 `start`는 생략하면 `0`이 되긴 하지만, 사실은 선택사항이 아니며 `undefined`가 `0`으로 변환되는 것 뿐이다. 공식 문서에서는 가급적 `0`으로 명시하라고 권장한다. 어쨋거나 `start`가 `0` 혹은 생략되면 전체 배열을 우측으로 시프트하는 것과 유사하게 작동한다:

```js
var array = ['a', 'b', 'c', 'd', 'e'];
array.copyWithin(1); // Array(5) [ "a", "a", "b", "c", "d" ]
array.copyWithin(2); // Array(5) [ "a", "b", "a", "b", "c" ]
array.copyWithin(3); // Array(5) [ "a", "b", "c", "a", "b" ]
array.copyWithin(4); // Array(5) [ "a", "b", "c", "d", "a" ]
```

### Array.prototype.entries()

배열의 각 인덱스에 대한 키와 값의 쌍(key-value pair)을 갖는 Array iterator 객체를 반환하는 메서드.

```
array.entries()
```

```js
var arr = ['a', 'b', 'c'];
var iter = arr.entries();
console.log(iter.next().value); // Array [ 0, "a" ]
console.log(iter.next().value); // Array [ 1, "b" ]
console.log(iter.next().value); // Array [ 2, "c" ]
```

보통은 `for-of`와 함께 쓴다:

```js
var arr = ['a', 'b', 'c'];
for (const [index, element] of arr.entries()) {
  console.log('index: ', index, ', element:', element);
}
// index:  0 , element: a
// index:  1 , element: b
// index:  2 , element: c
```

### Array.prototype.every()

```
array.every(callbackFn)
array.every(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

`forEach()`와 같이 배열의 모든 요소를 콜백 함수 첫 번째 매개변수로 제공한다. 다만 모든 반복에서 `return true`면 `true`, 하나라도 `return false`가 되면 `false`를 반환하는 특징을 이용해서 모든 요소가 특정 테스트를 통과하는지 확인할 때 사용한다.

```js
[1, 2, 3].every(ele => isFinite(ele)); // true
```

예를 들어 다음의 경우:

```js
// 코드 출처: https://bobbyhadz.com/blog/javascript-check-if-all-object-properties-are-null
var obj = {
  a: null,
  b: null
};
var isNullish = Object.values(obj).every(value => {
  if (value === null) {
    return true;
  }
  return false;
});
console.log(isNullish); // true
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

### Array.prototype.fill()

배열의 특정 인덱스를 주어진 값 하나로 채우거나 뒤바꾸는 메서드.

```
array.fill(value)
array.fill(value, start)
array.fill(value, start, end)
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

### Array.prototype.filter()

특정 조건으로 필터링한 새 배열을 반환한다.

```
array.filter(callbackFn)
array.filter(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

- `callbackFn`: 배열의 각 요소마다 실행될 콜백 함수. `true` 혹은 `false`를 반환해야 함
  - `element`: 배열의 각 요소
  - `index`: 인덱스
  - `array`: 배열 전체

콜백 함수에서 반환한 값이 `true`가 아닐 경우 해당 요소를 제외한 배열을 반환한다. 원본은 변하지 않는다.

```js
var arr = [1, 2, 3, 4];
var even = arr.filter(function (element) {
  return element % 2 == 0;
});
even; // [2, 4]
arr === even; // false
```

아래는 배열에서 중복 요소를 제거하는 코드다:

```js
array.filter((element, idx, sourceArray) => {
  return sourceArray.indexOf(element) !== sourceArray.lastIndexOf(element); 
});
```

앞부터 찾은 `element`와 뒤부터 찾은 `element`가 다르면 `sourceArray`에 중복된 `element`가 있다는 뜻이다. 따라서 false를 반환하여 sourceArray에서 제거한다.

### Array.prototype.find()

제공된 테스트 함수(provided testing function)를 만족(`true` 반환)하는 요소를 찾아 반환한다.

```
array.find(callbackFn)
array.find(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

- `callbackFn`: 배열의 각 요소마다 실행될 콜백 함수. `true` 혹은 `false`를 반환해야 함
  - `element`: 
  - `index`: 
  - `array`: 
- `thisArg`: 콜백 함수에서 `this`로 사용할 객체를 지정한다. 추가 내용은 [여기](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods)를 볼 것

콜백 함수에 요소를 전달하며 배열의 길이만큼 반복 실행한다. 콜백 함수의 반환값이 `true`면, 그 시점의 `element`를 반환하며 반복은 중단된다. 만약 모든 반복에서 `true`를 반환하지 않으면 최종 반환값은 `undefined`다.

```js
var arr = ['a', 'b', 'c'];
arr.find(ele => {
  return ele.charCodeAt() === 98; // 'c'는 반복에서 생략됨
});
// 'b'

arr.find(ele => {
  return ele.charCodeAt() === 101; // 101은 'e'인데 배열에 없으므로 undefined 반환
});
// undefined
```

ℹ️ `find()`, `map()`, `filter()` 처럼 함수를 인자로 받거나 반환하면 고차 함수(higher-order functions)라고 한다.

### Array.prototype.findIndex()

제공된 테스트 함수를 만족하는 요소의 인덱스를 반환한다.

```
array.findIndex(callbackFn)
array.findIndex(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

`find()`와 같으나 반환하는 값이 배열 요소의 인덱스라는 점만 다르다. 만약 모든 반복에서 `true`를 반환하지 않으면 최종 반환값은 `-1`이다.

```js
var arr3 = ['a', 'b', 'c'];
arr.findIndex(ele => {
  return ele == 'b'; // 'c'는 생략됨
})
// 1 

var arr4 = ['a', 'b', 'c'];
arr.findIndex(ele => {
  return ele == 'e';
});
// -1
```

### Array.prototype.findLast()

제공된 테스트 함수를 만족하는 요소를 배열의 끝에서 역순으로 찾아 반환한다. 나머지는 `find()`와 같다.

```
array.findLast(callbackFn)
array.findLast(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

```js
var arr = [1, 2, 3, 4, 5];
arr.findLast(ele => (ele < 5)); // 4
arr.findLast(ele => (ele < 2)); // 1
arr.findLast(ele => (ele < 1)); // undefined
```

### Array.prototype.findLastIndex()

제공된 테스트 함수를 만족하는 요소를 배열의 끝에서 역순으로 찾아 인덱스(역순으로 계산한 인덱스 아님. 원래 인덱스)의 반환한다. 나머지는 `findIndex()`와 같다.

```
array.findLastIndex(callbackFn)
array.findLastIndex(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

```js
var numbers = [5, 12, 8, 130, 44];
numbers.findLastIndex((number) => number < 20); // 2
numbers.findLastIndex((number) => number < 50); // 4
```

### Array.prototype.flat()

평탄화(?) 메서드. 배열의 내부에 중첩된 배열이 있는 경우, 중첩이 제거된(동일한 깊이의) 새 배열을 반환한다.

```js
array.flat()
array.flat(depth)
```

- `depth`: 얼마나 깊게 평탄화 해야 하는지 지정하는 옵션. 기본값은 1

```js
var arr1 = [1, 2, [3, [4, [5]]]];
arr1.flat(0); // [ 1, 2, [3, [4, [5]]] ];
arr1.flat(1); // [ 1, 2, 3, [4, [5]] ]
arr1.flat(2); // [ 1, 2, 3, 4, [5] ]
arr1.flat(3); // [ 1, 2, 3, 4, 5 ]
```

### Array.prototype.flatMap()

배열의 각 요소에 제공된 함수를 적용한 다음, 한 단계씩 평탄화된 새 배열을 반환하는 메서드. `flat(1)`과 `map()`을 합친 것과 비슷하다.

```
array.flatMap(callbackFn)
array.flatMap(callbackFn, thisArg)

function callbackFn(element, index, object) {}
```

- `callbackFn`: 새로운 배열 요소를 생성하는 함수.
  - `element`: 처리할 현재 요소.
  - `index`: 처리할 현재 요소의 인덱스.
  - `array`: flatMap을 호출한 배열.
- `thisArg`: `callback` 함수 내부에서 사용될 `this` 값.

```js
var arr = [[1], [2, 3], [3, 4, 5]];
arr.flatMap(e => e); // Array(6) [ 1, 2, 3, 3, 4, 5 ]

var numbers1 = [1, 2, 3, 4];
var result = numbers1.flatMap(num => [num, num * 2]);
result; // Array(8) [ 1, 2, 2, 4, 3, 6, 4, 8 ]

var sentences = ["안녕하세요 여러분", "JavaScript를 배우세요"];
var words = sentences.flatMap(sentence => sentence.split(" "));
words; // Array(4) [ "안녕하세요", "여러분", "JavaScript를", "배우세요" ]

var numbers2 = [1, 2, 3, 4, 5];
var evenNumbers = numbers2.flatMap(num => (num % 2 === 0 ? [num] : []));
evenNumbers; // Array [ 2, 4 ]
```

### Array.prototype.forEach()

배열의 모든 요소마다 반복하며 제공된 함수를 실행한다.

```
array.forEach(callbackFn)
array.forEach(callbackFn, thisArg)

function callbackFn(element, index, object) {}
```

`forEach()`는 `continue`와 `break` 사용이 불가능한데, `continue`는 `return`으로 구현할 수 있다. 하지만 `break`는 방법이 없으니(`return false`를 반환해 멈추는 건 jQuery에서나 가능했던 방법), 필요한 경우엔 for statement를 쓰도록 하자.

### Array.prototype.includes()

배열에 특정 요소가 있는지 판단하여 `true`/`false`를 반환한다.

```
includes(searchElement)
includes(searchElement, fromIndex)
```

- `searchElement`: 찾을 값
- `fromIndex`: 검색을 시작할 zero-based 인덱스. 생략하면 기본값은 `0`이며, 음수로 지정하면 검색 시작 위치를 거꾸로 계산한다(끝에서 첫 번째는 `-1`, 끝에서 두 번째는 `-2`). 이 값이 음수라고 해서 검색 방향이 바뀌는 것은 아님.

```js
var arr = ['a', 'b', 'c'];
arr.includes('b'); // true
arr.includes('d'); // false
arr.includes('a', 0); // true
arr.includes('a', 1); // false
arr.includes('a', 2); // false
arr.includes('a', -1); // false
arr.includes('a', -2); // false
arr.includes('a', -3); // true
```

검색에는 [SameValueZero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness#same-value-zero_equality) 알고리즘을 사용한다.

### Array.prototype.indexOf()

배열에서 특정 요소가 처음으로 나타나는 인덱스를 반환한다. 찾으려는 요소가 배열에 존재하지 않으면 `-1`을 반환한다.

```
array.indexOf(searchElement)
array.indexOf(searchElement, fromIndex)
```

- `searchElement`: 찾으려는 요소
- `fromIndex`: 검색을 시작할 인덱스. 생략하면 기본값은 `0`이다. 이 값이 음수일 경우 인덱스를 배열의 끝에서부터 계산한다. 예를 들어 `-1`은 배열의 마지막 요소를 의미하며, 마지막 요소가 찾는 요소인지만 확인하게 된다. 그리고 이 값이 어찌됐든 검색은 앞에서 뒤로 진행한다.

```js
var arr = [2, 5, 9, 2];

arr.indexOf(9); // 2
arr.indexOf(2); // 0
arr.indexOf(7); // -1
arr.indexOf(2, 1); // 3 (1번 인덱스 이후에서 2를 찾음)
arr.indexOf(9, -3) // 2 (끝에서 세 번째 요소부터 검색 시작)
```

### Array.prototype.join()

배열의 모든 요소를 하나의 문자열로 결합하여 반환한다.

```
array.join()
array.join(separator)
```

- `separator`: 구분자로 사용할 문자열. 생략하면 쉼표`,`가 사용된다.

```js
var arr = ['a', 'b', 'c'];
arr.join(); // "a,b,c"
arr.join(''); // "abc"
arr.join(', '); // "a, b, c"
```

### Array.prototype.keys()

배열의 모든 인덱스를 담은 [이터레이터(Iterator)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator) 객체를 반환한다. 배열의 인덱스를 순회할 때 사용한다.

```
array.keys()
```

```js
var arr = ['a', 'b', 'c'];
var iterator = arr.keys();

iterator.next(); // Object { value: 0, done: false }
iterator.next(); // Object { value: 1, done: false }
iterator.next(); // Object { value: 2, done: false }
iterator.next(); // Object { value: undefined, done: true }
```

`iterator.next()`가 반환하는 객체의 `done` 프로퍼티는 왜 꼭 네 번 째 호출에 `true`가 되는지 의문이다. 그 직전에 `true`를 반환해야 쓸데가 있을 거 같은데...

### Array.prototype.lastIndexOf()

배열에서 특정 요소가 마지막으로 나타나는 위치의 인덱스를 반환한다. `indexOf()`와 다르게 배열의 끝에서 앞 방향으로 찾는다. 만약 찾으려는 요소가 배열에 존재하지 않으면 `-1`을 반환한다.

```
array.lastIndexOf(searchElement)
array.lastIndexOf(searchElement, fromIndex)
```

- `searchElement`: 찾으려는 요소
- `fromIndex`: 역방향 검색을 시작할 zero-based 인덱스. 생략하면 배열의 마지막 인덱스(`array.length - 1`)가 사용되며 배열 전체에서 찾는다. 이 값이 음수일 경우 인덱스를 배열의 끝에서부터 계산한다. 예를 들어 `-1`은 배열의 마지막 인덱스이며 마지막 요소도 포함하여 찾는다.

```js
var arr = ['a', 'b', 'c', 'd', 'b'];

// fromIndex를 지정하지 않으면 마지막 인덱스(4)에서 시작
arr.lastIndexOf('b'); // 4

// 인덱스 2('c')부터 역방향으로 'b'를 검색
arr.lastIndexOf('b', 2); // 1

// fromIndex가 음수인 경우: -1이면 마지막 인덱스(4)부터 시작
arr.lastIndexOf('b', -1); // 4

// fromIndex = -3이면 arr.length - 3 = 5 - 3 = 2 인덱스부터 시작
arr.lastIndexOf('b', -3); // 1
```

`indexOf()`와 조합하여 배열에 중복 요소가 존재하는지 판단하는 데 활용할 수 있다:

```js
array.filter((element, idx, sourceArray) => {
  return sourceArray.indexOf(element) !== sourceArray.lastIndexOf(element); 
});
```

### Array.prototype.map()

```
array.map(callbackFn)
array.map(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

- `thisArg`: 콜백 함수를 실행할 때 `this`로 간주할 객체

주어진 함수가 반환하는 결과로 새로운 배열을 생성해 반환.

```js
[1, 2, 3, 4, 5].map(n => n * 2); // [ 2, 4, 6, 8, 10 ]

[1, 2, 3, 4, 5].map(function (n) {
  return n == 1;
});
// [ true, false, false, false, false ]
```

### Array.prototype.pop()

맨 뒤 요소를 반환하고 **원본 배열에서 제거**한다.

```js
var arr = ['a', 'b', 'c'];
arr.pop(); // "c"
arr; // Array [ "a", "b" ]
```

### Array.prototype.push()

배열의 맨 뒤에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.push('a'); // 1
arr.push('b', 'c', 'd'); // 4
arr; // ['a', 'b', 'c', 'd']
```

### Array.prototype.reduce()

배열의 처음부터 마지막까지 순회하며, 각 반복의 콜백 함수가 반환한 값을 다음 반복으로 넘겨주는 함수다. 왠지 뭔가 감소할 것 같은 메서드 이름과 다르게, 원본 배열은 변하지 않는다.

```
array.reduce(callbackFn)
array.reduce(callbackFn, initialValue)

function callbackFn(accumulator, currentValue, currentIndex, array) {}
```

이전 요소의 연산결과를 넘겨준다.

- `initialValue`: `accumulator`의 초깃값. 생략하면 배열의 첫 번째 요소가 사용된다.
- `callbackFn`
  - `accumulator`: 이전 콜백 함수 호출의 반환값
  - `currentValue`: 현재 순회 중인 배열 요소
  - `currentIndex`: 현재 요소의 인덱스
  - `array`: 원본 배열

이 함수는 주로 배열의 모든 요소를 순회하여 연산한 결과값이 필요할 때 사용한다:

```js
// 숫자 배열의 합계 구하기
var arr = [1, 2, 3, 4];
arr.reduce((prev, cur) => prev + cur); // 10

var arr2 = [1, 2, 3, 4];
arr2.reduce((prev, cur) => prev + cur, 20); // 초깃값 20에 10을 더한 30이 반환됨
```

`reduce()` 메서드는 배열의 왼쪽 요소부터 오른쪽으로 순회한다. 반대로, 오른쪽에서 왼쪽으로 순회하는 메서드인 `Array.prototype.reduceRight()`도 있다.

### Array.prototype.reduceRight()

배열의 마지막 요소에서 첫 번째 요소 방향으로 순회하며, 각 반복의 콜백 함수가 반환한 값을 다음 반복으로 넘겨주는 함수다. `Array.prototype.reduce()`와 순회 방향이 반대라는 점만 빼고 같다.

```
reduceRight(callbackFn)
reduceRight(callbackFn, initialValue)

function callbackFn(accumulator, currentValue, currentIndex, array) {}
```

- `initialValue`: `accumulator`의 초깃값. 생략하면 배열의 마지막 요소가 사용된다.
- `callbackFn`
  - `accumulator`: 이전 콜백 함수 호출의 반환값
  - `currentValue`: 현재 순회 중인 배열 요소
  - `currentIndex`: 현재 요소의 인덱스
  - `array`: 원본 배열

```js
var words = ['world', 'hello'];
words.reduceRight((acc, word) => acc + ' ' + word); // "hello world"

var nested = [[0, 1], [2, 3], [4, 5]];
nested.reduceRight((acc, curr) => acc.concat(curr), []); // Array(6) [ 4, 5, 2, 3, 0, 1 ]
```

### Array.prototype.reverse()

배열의 배치를 거꾸로 뒤집는 메서드. **원본이 변한다.** 원본을 유지하려면 `toReversed()`를 쓰자.

```js
var arr = [1, 2, 3, 4];
var reversed = arr.reverse();
reversed; // Array(4) [ 4, 3, 2, 1 ]
arr; // Array(4) [ 4, 3, 2, 1 ]
```

메서드 매개변수는 음슴.

### Array.prototype.shift()

맨 앞 요소를 반환하고 **원본 배열에서 제거**한다.

```js
var arr = ['a', 'b', 'c'];
arr.shift(); // "a"
arr; // Array [ "b", "c" ]
```

⚠️ `shift()`도 `unshift()`와 같이 배열의 크기가 커질수록 연산 비용이 증가한다.

### Array.prototype.slice()

```
array.slice(begin)
array.slice(begin, end)
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

### Array.prototype.some()

배열의 요소 중 하나라도 주어진 테스트를 통과하면 `true`를 반환하는 함수. 특정 요소의 존재 여부를 확인하는 용도로 가장 적합하다.

```
array.some(callbackFn)
array.some(callbackFn, thisArg)

function callbackFn(element, index, array) {}
```

`every()`와 비슷하지만 반대로 작동한다. 모든 반복의 콜백 함수 중 하나라도 `true`를 반환하면 최종 결과는 `true`다:

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

### Array.prototype.sort()

그냥 배열을 정렬해주는 평범한 메서드. **원본이 변한다.** 원본을 유지하려면 `toSorted()`를 쓰도록.

```
array.sort()
array.sort(compareFn)

function compareFn(a, b) {}
```

`compareFn`을 생략하면 오름차순으로 알아서 해주는데, 놀랍게도 다중배열에도 쓸 수 있다:

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

### Array.prototype.splice()

주어진 인덱스에 따라 일부 요소를 잘라내 반환한다.

```
array.splice(start)
array.splice(start, deleteCount)
array.splice(start, deleteCount, item1, item2, ...)
```

- `start`: 잘라내기 시작 인덱스
- `deleteCount`: 잘라낼 요소의 수
- `item...`: 원본배열에 덧붙일 요소

잘라낸 요소는 **원본 배열에서 사라진다**. 원본을 유지하려면 `toSpliced()`를 쓰자.

```js
var arr = ['a', 'b', 'c'];
arr.splice(0); // Array(3) [ "a", "b", "c" ]
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

### Array.prototype.toReversed()

원본 배열의 순서를 뒤집은 새 배열을 반환한다. 원본이 변하지 않으며 복사본을 반환한다는 점을 제외하면 `reverse()`와 같다.

```js
var arr = ['a', 'b', 'c'];
arr.toReversed(); // Array(3) [ "c", "b", "a" ]
arr; // Array(3) [ "a", "b", "c" ]
```

### Array.prototype.toSorted()

배열을 오름차순으로 정렬한다. 원본이 변하지 않으며 복사본을 반환한다는 점을 제외하면 `sort()`와 같다.

```
array.toSorted()
array.toSorted(compareFn)

function compareFn(a, b) {}
```

- `compareFn`: 정렬 순서를 정의하는 함수를 지정하는 콜백 함수. 이 함수가 음수를 반환하면 `a`가 `b`보다 앞으로, 양수를 바노한하면 `b`가 `a`보다 앞으로 정렬되며, 0이나 `NaN`을 반환하면 동등하다 판단하여 정렬하지 않는다. 이 함수를 생략하면 배열의 각 요소를 문자열로 변환하여 유니코드 코드 포인트 값에 따라 오름차순 정렬한다.

```js
var arr1 = ['b', 'a', 'd', 'c'];
arr1.toSorted(); // [ "a", "b", "c", "d" ]
arr1; // [ "b", "a", "d", "c" ]
```

`compareFn` 콜백 함수를 제공하면 정렬 순서를 마음대로 조정할 수 있음:

```js
var arr1 = ['b', 'a', 'd', 'c'];

// 오름차순 정렬
arr1.toSorted((a, b) => {
  return a.charCodeAt(0) - b.charCodeAt(0)
});
// [ "a", "b", "c", "d" ]

// 내림차순 정렬
arr1.toSorted((a, b) => {
  return b.charCodeAt(0) - a.charCodeAt(0)
});
// [ "d", "c", "b", "a" ]

var arr2 = [1, 2, 3, 4, 5, 6, 7, 8];

// 홀수를 좌측, 짝수를 우측으로
arr2.toSorted((a, b) => {
  return b % 2
});
// [ 7, 5, 3, 1, 2, 4, 6, 8 ]
```

### Array.prototype.toSpliced()

주어진 인덱스에 따라 일부 요소를 새 배열에 복사하여 반환한다. `splice()` 메서드와 다르게 원본 배열은 변하지 않으며, 지정한 인덱스의 요소를 제외한 나머지의 배열을 반환한다. `slice()`와 다르게 반환할 배열에 덧붙일 추가 인자를 받는다.

```
array.toSpliced(start)
array.toSpliced(start, deleteCount)
array.toSpliced(start, deleteCount, item1, item2, ...)
```

- `start`: 잘라내기를 시작할 위치(인덱스)
- `deleteCount`: 잘라낼 요소의 수
- `item...`: 반환할 배열에 덧붙일 요소

```js
var arr1 = ['a', 'b', 'c'];
arr1.toSpliced(0, 1); // 'a'를 제외한 ['b', 'c']
arr1.toSpliced(1, 2); // 'b'와 'c'를 제외한 ['a']
arr1.toSpliced(1, 2, 8, 9); // 'b'와 'c'를 제외하고 8, 9를 덧붙인 ['a', 8, 9]
```

### Array.prototype.unshift()

`push()`와 반대로 배열의 맨 앞에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.unshift('a'); // 1
arr.unshift('b', 'c', 'd'); // 4
arr; // ['b', 'c', 'd', 'a']
```

⚠️ `unshift()` 연산은 배열의 모든 요소를 원래의 자리에서 이동시키기 때문에, 배열의 크기가 커질수록 연산 비용이 증가한다.

### Array.prototype.with()

배열에서 특정 요소 하나를 수정한 새 배열을 반환한다. 원본은 변하지 않는다. ES2022의 채신 기능인데... 이걸 어따 쓰지? 😒

```
arrayInstance.with(index, value)
```

- `index`: 변경할 요소의 인덱스
- `value`: 해당 인덱스에 새로 설정할 값

```js
var array = [1, 2, 3, 4, 5];
array.with(2, 99); // Array(5) [ 1, 2, 99, 4, 5 ]
```
