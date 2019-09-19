---
layout: post
date: 2019-03-29 00:00:00 +0900
title: 'JavaScript: Set'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - set
  - standard built-in objects
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)

Standard built-in Objects: Set

Array와 비슷하지만 완전히 같지는 않은 타입. 내부 배열 내에서 유일한 값만 추가할 수 있다.

```js
let b = new Set();

b.add(1);
b.size; // 1
b.add(1);
b.size; // 1
b.add(1);
b.size; // 여전히 1. 1은 이미 있어서 더 이상 배열에 추가할 수 없다.

b.add(2);
b.size; // 2

b; // Set(2) {1, 2}
```
