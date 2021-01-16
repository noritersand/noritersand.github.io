---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 전개 구문 Spread syntax (...)'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - es2015
  - shorthand
  - computed
  - property-names
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#전개_속성](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#전개_속성)

#### 버전 정보

- ES2015에서 최초 정의
- Chrome 60/Edge 79/Firefox 55/Opera 47/Safari 11.1 이상에서 사용 가능
- IE에서 사용 불가

전개 구문은 객체나 배열의 요소를 말그대로 전개하거나 분해할 때 사용한다. 단순 전개부터 `apply()`를 대체하거나 `new`에 적용, 배열 복사, 배열 연결, 객체 복사 등에 사용할 수 있다.

```js
var n = [1, 2, 3];
console.log(n); // Array(3) [ 1, 2, 3 ]
console.log(...n); // 1 2 3
```

여기서 점 세개`...`는 전개 연산자<sup>spread operator</sup>라고 부른다.

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
