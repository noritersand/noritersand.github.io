---
layout: post
date: 2023-12-17 15:26:18 +0900
title: '[JavaScript] Set'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - set
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN | Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)


## 개요

표준 내장 객체 `Set`에 대해서 정리한 글.

`Set`은 중복을 허용하지 않는 집합을 나타내는 데이터 구조다. 형태는 배열과 비슷하지만(실제로 유사 배열로 취급된다) 중복 값이 자동으로 제거되는 특징을 이용해 집합 연산이나 필터링 등에서 사용한다.

수학의 집합을 표현하기 좋은 데이터 구조이며 집합 연산(교집합, 합집합, 차집합)에 사용하기도 수월하다.

```js
var st = new Set();
st.add(1);
st.add(1);
st; // Set [ 1 ]

var st2 = new Set([1, 2, 2, 3]);
st2; // Set(3) [ 1, 2, 3 ]

Array.from(st); // Array(3) [ 1, 2, 3 ]
```

특이하게도 `Set`은 값의 유일성과 순서를 위한 데이터 구조라서 그런지 getter 류의 메서드가 존재하지 않는다.


## 요소가 객체일 때의 중복 제거

요소가 객체일 때는, 객체 내부의 값이 아니라 객체 자체의 참조값을 기준으로 중복을 제거한다.

예를 들어 다음과 같은 코드를 실행하면:

```javascript
const set = new Set([[1], [1]]);
console.log([...set]);
```

결과는 `[[1], [1]]`이 된다.

배열의 중복을 제거하려면 다른 방법을 사용해야 한다. 예를 들어 `JSON.stringify`와 `JSON.parse` 함수를 사용하여 배열을 문자열로 변환한 후 중복을 제거하고 다시 배열로 변환하는 방법이 있다:

```javascript
const arr = [[1], [1]];
const set = new Set(arr.map(JSON.stringify));
const result = [...set].map(JSON.parse);
result; // [[1]]
```


## 인스턴스 프로퍼티

몇 개의 요소가 있는지를 반환하는 `Set.prototype.size` 하나 있다.


## 인스턴스 메서드

### Set.prototype.add()

인스턴스에 새 요소를 (이미 동일한 값의 요소가 없는 경우에 한하여) 추가한다.

```js
var mySet = new Set([1, 2, 3]);
mySet.add(1);
mySet.add(2);
mySet.add(3);
mySet.add(2); // 이 값은 무시됨

console.log(mySet); // Set(3) { 1, 2, 3 }
```

### Set.prototype.clear()

`Set` 인스턴스의 모든 요소를 제거한다.

```js
var mySet = new Set([1, 2, 3]);

console.log(mySet.size); // 3
mySet.clear();
console.log(mySet.size); // 0
```

### Set.prototype.delete()

지정된 값이 인스턴스에 있는 경우 해당 값을 제거한다.

```js
var mySet = new Set([1, 2, 3]);

mySet.delete(2);
console.log(mySet.size); // 2
```

### Set.prototype.has()

특징 값이 인스턴스에 있는지를 `boolean` 타입으로 반환한다.

```js
var mySet = new Set([1, 2, 3]);
console.log(mySet.has(3)); // true
```

### Set.prototype.entries()

Set 인스턴스의 항목() 메서드는 삽입 순서대로 이 집합의 각 요소에 대한 [값, 값] 배열을 포함하는 새로운 집합 반복자 개체를 반환합니다. Set 객체의 경우 Map 객체와 같은 키가 없습니다. 그러나 API를 Map 객체와 유사하게 유지하기 위해 각 항목은 여기에서 키와 값에 대해 동일한 값을 가지므로 [값, 값] 배열이 반환됩니다.

이 메서드는 `Set` 인스턴스에 추가된 순서 그대로의 `[값, 값]` 배열을 포함하는 이터레이터(iterator)를, 호출할 때마다 새로 만들어 반환한다.

키 자리에는 값을 대신 채운 특이한 형태인데, `Map`과의 유사성을 유지하기 위함이라나 뭐라나...

```js
var mySet = new Set([1, 2, 3]);
console.log(mySet.entries());
// Set Iterator { [ 1, 1 ], [ 2, 2 ], [ 3, 3 ] }
```

### Set.prototype.values()

`Set` 인스턴스 요소들의 값만을 포함하는 이터레이터를 반환한다.

```js
var mySet = new Set([1, 2, 3]);
console.log(mySet.values()); // Set Iterator { 1, 2, 3 }
```

### Set.prototype.keys()

이 메서드는 `Set.prototype.values()`의 별칭이다.

### Set.prototype.forEach()

```
set.forEach(callbackFn)
set.forEach(callbackFn, thisArg)

function callbackFn(value, key, set) {}
```

인스턴스의 요소가 추가된 순서대로 반복하며 제공된 함수를 호출한다. 다른 메서드들이 그런것 처럼, 키가 없기 때문에 `key`에는 값이 할당된다.

```js
var mySet = new Set([1, 2, 3]);

mySet.forEach((value, key, set) => {
  console.log('value:', value);
  console.log('key:', key);
  console.log('set:', set);
});

// value: 1
// key: 1
// set: Set(3) { 1, 2, 3 }

// value: 2
// key: 2
// set: Set(3) { 1, 2, 3 }

// value: 3
// key: 3
// set: Set(3) { 1, 2, 3 }
```


## 이 아래부터는 실험적 기능이므로 지원하는 브라우저를 확인할 것

### Set.prototype.difference()

**TODO**

### Set.prototype.intersection()

**TODO**

### Set.prototype.isDisjointFrom()

**TODO**

### Set.prototype.isSubsetOf()

**TODO**

### Set.prototype.isSupersetOf()

**TODO**

### Set.prototype.symmetricDifference()

**TODO**

### Set.prototype.union()

**TODO**


끗.
