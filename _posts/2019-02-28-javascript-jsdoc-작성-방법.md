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

#### 관련 문서

- [@use JSDoc](https://jsdoc.app/)
- [https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description)
- [https://stackoverflow.com/questions/8407622/set-type-for-function-parameters](https://stackoverflow.com/questions/8407622/set-type-for-function-parameters)


## 개요

JSDoc에 대한 간단 정리글.


## doc가 뭔데

함수나 메서드, 프로토타입, (자바의) 클래스 등 어떤 것을 선언할 때 선언부 바로 상단에 위치하는 코멘트 블록(multiline comment)을 말한다. 관례 상 일반적인 코멘트 블록과 다르게 `/**`로 시작한다. 작성해두면 IDE에서 툴팁으로 코멘트 내용을 미리 보여준다던지 등의 여러모로 도움이 됨. (자바는 이예 문서로 만들어주고...)

doc 자체를 부르려고 할 때, 자바스크립트는 JSDoc(사실 이것은 [Nodejs 패키지](https://github.com/jsdoc/jsdoc)다), 자바는 Javadoc이라 하면 되는데, 앞의 언어는 때고 doc이라고 쓰면 의미가 불분명하다.

우리말로는 '문서 코멘트'가 적당하고, 영문으로는:

- documentation comments (인텔리제이를 참고함)
- doc comments

이 정도로 늘려쓰면 될 것 같음.


## 작성방법

```js
/**
 * @param {Date} myDate The date
 * @param {string} myString The string
 * @param {boolean} myFlag The flag
 * @param {string|number} myArg I dunno
 * @returns {string|Object} return string or number
 */
function myFunction(myDate, myString, myFlag, myArg) {
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


## 데이터 타입

부수효과로 편집기에 따라 툴팁으로 파라미터의 타입이 보인다거나, 자동 완성 기능이 타입에 맞춰서 알아서 된다거나 하는게 있다.

구성요소의 타입을 특정할 수 없는 배열이면 `Array` 혹은 `any[]`라고 적는다. 반대의 경우엔 해당 타입과 대괄호`[]`를 적는다. 가령 `string` 타입의 배열이면 `string[]`이다.

자동완성되는 걸 보면 이런 것도 가능:

```
@returns {Promise<void>}
```

원시 타입(`number`, `string`, `boolean`, `symbol`)을 제외한 나머지는 프로토타입을 써주면 된다. e.g., `Object`, `Function`, `Document`, `Node`, `Window`, ...

`*`는 모든 타입이 올 수 있음을 나타낸다.

### OR

여러 타입을 허용하는 경우 이렇게 쓴다:

```js
/**
 * @returns {null|string|*} null 혹은 string 혹은 any를 반환한다는 뜻 (사실상 쓰나마나다 🤭)
 */
function getAny() {
  //... 
}
```


## 파일 JSDoc

메서드나 변수에 대한 코멘트 말고 파일 상단에 작성하는 코멘트는 정해진 규칙이 따로 없지만, 대게 이렇게 시작한다:

```js
/*!
 * 이 편지는 영국에서 최초로 시작되어 일년에
 * 한바퀴를 돌면서 받는 사람에게 행운을 주었고...
 */
```


## 구조 분해 할당을 사용하는 함수 파라미터는?

```js
// 소스 출처: https://stackoverflow.com/questions/36916790/document-destructured-function-parameter-in-jsdoc
/**
 * My cool function.
 *
 * @param {Object} obj - An object.
 * @param {string} obj.prop1 - Property 1.
 * @param {string} obj.prop2 - Property 2.
 */
function willDestroyU({prop1, prop2}) {
  // Do something with prop1 and prop2
}
```

스펙 정의는 [여기](https://jsdoc.app/tags-param.html#parameters-with-properties)를 참고.
