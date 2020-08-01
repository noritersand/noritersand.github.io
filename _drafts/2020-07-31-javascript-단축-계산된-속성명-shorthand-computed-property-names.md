---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 단축/계산된 속성명 Shorthand/Computed property names'
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

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)

```js
// 단축 속성명 (ES6)
var a = "foo", b = 42, c = {};
var o = { a, b, c };
```

```js
// 계산된 속성명 (ES6)
var prop = "foo";
var o = {
  [prop]: "hey",
  ["b" + "ar"]: "there",
};
```
