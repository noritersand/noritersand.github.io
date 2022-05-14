---
layout: post
date: 2019-02-28 00:00:00 +0900
title: '[JavaScript] JSDoc 작성 방법'
categories:
  - javascript
tags:
  - javascript
  - jsdoc
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [@use JSDoc](https://jsdoc.app/)
- [https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description)
- [https://stackoverflow.com/questions/8407622/set-type-for-function-parameters](https://stackoverflow.com/questions/8407622/set-type-for-function-parameters)

## 개요

JSDoc 어쩌구 설명 TODO...

```js
/**
 * @param {Date} myDate The date
 * @param {string} myString The string
 * @param {string|number} myArg i dunno
 * @returns {string|Object} return string or number
 */
function myFunction(myDate, myString, myArg) {
  // do stuff
}

/**
 * Returns the sum of a and b
 * @param {number} a
 * @param {number} b
 * @returns {Promise} Promise object represents the sum of a and b
 */
function sumAsync(a, b) {
  return Promise.resolve(a + b);
}
```

요딴식으로 작성하면 됨.

부수효과로 편집기에 따라 툴팁으로 파라미터의 타입이 보인다거나, 자동 완성 기능이 타입에 맞춰서 알아서 된다거나 하는게 있다.

구성요소의 타입을 특정할 수 없는 배열이면 `array`라고 적는다. 이 경우 `any[]`로 나타난다. 반대의 경우엔 해당 타입과 대괄호`[]`를 적는다. 가령 `string` 타입의 배열이면 `string[]`이다.

인텔리제이에서 만들어주는 걸 보니 타입만 정의하는게 아니라 객체의 프로퍼티를 나열하는 방식도 되나보다. (예시는 지킬 빌드가 안되서 생략함)

## 여담: 파일 JSDoc

메서드나 변수에 대한 코멘트 말고 파일 상단에 작성하는 코멘트는 정해진 규칙이 따로 없긴 하지만, 보통 이렇게 시작한다:

```js
/*!
 * 이 편지는 영국에서 최초로 시작되어 일년에
 * 한바퀴를 돌면서 받는 사람에게 행운을 주었고...
 */
```

## 여담2: doc을 뭘로 번역해야 하나

JSDoc, Javadoc이라 쓰면 되긴 한데 언어 때고 doc이라고 쓰면 의미가 불분명함... 인텔리제이를 보니 'documentation comments'라고 지칭하고 있음. (혹은 'doc comments') 따라서 우리말로는 '문서 코멘트' 정도로 번역할 수 있겠다.

## 꼐속...
