---
layout: post
date: 2019-02-28 00:00:00 +0900
title: '[JavaScript] JSDoc 작성 방법'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - jsdoc
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[JSDoc\] Getting Started with JSDoc 3](https://jsdoc.app/about-getting-started.html)
- [https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description)
- [https://stackoverflow.com/questions/8407622/set-type-for-function-parameters](https://stackoverflow.com/questions/8407622/set-type-for-function-parameters)

## 개요

JSDoc 어쩌구 설명 TODO...

## 함수 파라미터의 타입 설정

```js
/**
 * @param {Date} myDate The date
 * @param {string} myString The string
 * @param {string|number} myArg i dunno
 * @return {string} return string
 */
function myFunction(myDate, myString, myArg) {
    //do stuff
}
```

요딴식으로 작성하면 됨.

부수효과로 편집기에 따라 툴팁으로 파라미터의 타입이 보인다거나, 자동 완성 기능이 타입에 맞춰서 알아서 된다거나 하는게 있다.

구성요소의 타입을 특정할 수 없는 배열이면 `array`라고 적는다. 이 경우 `any[]`로 나타난다. 반대의 경우엔 해당 타입과 대괄호`[]`를 적는다. 가령 `string` 타입의 배열이면 `string[]`이다.

## 꼐속...
