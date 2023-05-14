---
layout: post
date: 2019-03-29 00:00:00 +0900
title: '[JavaScript] Set'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - set
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)


## 개요

빌트인 프로토타입인 Set에 대해서 정리한 글.

Set은 Array와 비슷하지만 완전히 같지는 않은 타입. 중복을 허용하지 않는 특징이 있다.

```js
var st = new Set();
st.add(1);
st.add(1);
st; // Set [ 1 ]

var st2 = new Set([1, 2, 2, 3]);
st2; // Set(3) [ 1, 2, 3 ]

Array.from(st2); // Array(3) [ 1, 2, 3 ]
```


## 요소가 객체일 때의 중복 제거

Set은 중복을 허용하지 않는 자료구조이지만, 요소가 객체일 때는 내부의 값이 같더라도 참조값이 다르기 때문에 중복 제거가 되지 않는다.

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


