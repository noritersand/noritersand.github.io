---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 계산된 속성명 Computed property names'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - computed-property-names
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer](https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)

```js
// 계산된 속성명 (ES6)
var prop = "foo";
var o = {
  [prop]: "hey",
  ["b" + "ar"]: "there",
};
```
