---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 전개 구문 Spread syntax (...)'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - shorthand
  - computed
  - property-names
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

ES2015에서 소개된 구문. 객체나 배열의 요소를 말그대로 전개하거나 분해할 때 사용한다.  
단순 전개부터 `apply()`를 대체하거나 `new`에 적용, 배열 복사, 배열 연결, 객체 복사 등에 사용할 수 있다.

```js
var n = [1, 2, 3];
console.log(n); // Array(3) [ 1, 2, 3 ]
console.log(...n); // 1 2 3
```

## 나머지 구문

구조 분해 할당 표현식에서 사용할 땐 전개 구문 대신 `나머지 구문`이라 부른다:

```js
var [min = 0, max = Infinity, ...rest] = [1, 2, 3, 4, 5];
min; // 1
max; // 2
rest; // [3, 4, 5]

var { x = 24, b = 48, ...y } = { a: 1, b: 2, c: 3, d: 4 };
x; // 24
b; // 2
y; // { a: 1, c: 3, d: 4 }
```
