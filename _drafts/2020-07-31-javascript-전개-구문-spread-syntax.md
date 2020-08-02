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

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6

console.log(sum.apply(null, numbers));
// expected output: 6
```
