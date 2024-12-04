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

#### 참고 문서

- [Use JSDoc \| Index](https://jsdoc.app/)
- [GitHub \| google/closure-compiler \| Annotating JavaScript for the Closure Compiler](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler#param-type-varname-description)
- [Stack Overflow \| javascript - Set type for function parameters?](https://stackoverflow.com/questions/8407622/set-type-for-function-parameters)


## 개요

JSDoc란 함수나 메서드, 프로토타입, (자바의) 클래스 등의 선언부(혹은 정의부) 바로 위에 작성하는 코멘트 블록(Multiline comment)을 말한다. 관례상 일반적인 코멘트 블록과 다르게 `/**`로 시작한다. 작성해두면 IDE에서 툴팁으로 코멘트 내용을 미리 보여준다던지 등의 소소한 이점도 있다.

간단히 말해서 자바의 Javadoc처럼 API 문서(API Documentation) 생성을 위한 특수한 코멘트 블록이라 할 수 있다.


## doc가 뭔데

우리말로는 '문서 코멘트' 정도로 부르면 된다.

영문으로는:

- Documentation comments (IntelliJ)
- Doc comments

정도로 쓰면 될 것 같음.


## JSDoc를 문서로 내보내기

[Node.js 패키지 JSDoc](https://github.com/jsdoc/jsdoc)를 설치해 실행하면 된다.

```bash
# JSDOc 설치
npm install --save-dev jsdoc

# 파일로 만들기
npm exec jsdoc ./src/test/js-doc-test.js
```

이렇게 하고 `out` 디렉터리를 확인해보자.


## 작성 방법

```js
/**
 * @param {Date} myDate The date
 * @param {string} myString The string
 * @param {boolean} myFlag The flag
 * @param {string|number} myArg I dunno
 * @returns {string|Object} return string or number
 */
function myFn(myDate, myString, myFlag, myArg) {
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

요딴식으로 하면 됨.


## 블록 태그와 인라인 태그

```js
/**
 * Set the shoe's color. Use {@link Shoe#setSize} to set the shoe size.
 *
 * @param {string} color - The shoe's color.
 */
Shoe.prototype.setColor = function (color) {
    // ...
};
```

`@param`이 블록 태그고 `{@link}`가 인라인 태그다. 현재(2024-02-19) 인라인 태그는 `@link`(별칭: `@linkcode`, `@linkplain`)와 `@tutorial`만 지원한다.


## 파일 doc

예전에는 대체로 이렇게 하는 경우도 많았는데:

```js
/*!
 * 이 편지는 영국에서 최초로 시작되어 일년에 한바퀴를 돌면서 받는 사람에게 행운을 주었고...
 */
```

[JSDoc](https://jsdoc.app/tags-file.html)에서는 `@file` 태그를 이용하라고 한다:

```js
/**
 * @file 이 편지는 영국에서 최초로 시작되어 일년에 한바퀴를 돌면서 받는 사람에게 행운을 주었고...
 * @since 2023-09-19
 */
```


## 데이터 타입

부수효과로 편집기에 따라 툴팁으로 파라미터의 타입이 보인다거나, 자동 완성 기능이 타입에 맞춰서 알아서 된다거나 하는게 있다.

구성 요소의 타입을 특정할 수 없는 배열이면 `Array` 혹은 `any[]`라고 적는다. 반대의 경우엔 해당 타입과 대괄호`[]`를 적는다. 가령 `string` 타입의 배열이면 `string[]`이다.

자동완성되는 걸 보면 이런 것도 가능:

```
@returns {Promise<void>}
```

원시 타입(`number`, `string`, `boolean`, `symbol`)을 제외한 나머지는 프로토타입 이름 그대로 써주면 된다. e.g., `Object`, `Function`, `Document`, `Node`, `Window`, ...

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

### 상수나 변수의 데이터 타입

`@type`을 쓴다.

```js
/**
 * 푸-바
 * 
 * @type {string}
 */
const foo = 'bar';
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


## 마크다운 플러그인 사용하기

JSDoc는 기본적으로 줄 바꿈을 지원하지 않아서, 줄 바꿈이나 목록을 표현하고 싶다면 HTML 태그를 강제로 사용하거나 마크다운 플러그인을 적용해야 한다.

```json
{
  "plugins": ["plugins/markdown"]
}
```

위처럼 작성된 설정 파일을 하나 만들고, 빌드할 때 옵션으로 해당 설정 파일의 경로를 지정해주면 끗:

```bash
# ./src에 있는 소스 파일을 대상으로 빌드. 빌드 경과는 ./doc 아래에 생성한다. 이 때 설정파일로 ./confi/jsdoc-config.json 파일을 사용함
npm jsdoc ./src --destination ./doc --configure ./conf/jsdoc-config.json
```

이렇게하면 코멘트를 마크다운 서식으로 인식한다.

```js
/**
 * @file 코파일럿 테스트용 파일
 * 
 * #### 단축키
 * 
 * - `alt + \`: 코파일럿 발동
 * - `tab`: 코파일럿 제안 선택
 * 
 * ...
 * function fn() {
 *  console.log('Hello, world!');
 * }
 * ```
 *
 * (생략)
 */
```


## 블록 태그들

### @description

별칭: `@desc`

함수나 변수의 설명을 작성할 때 사용한다. 이 태그를 명시하면 태그가 적용되지 않은 코멘트는 무시된다(`@file`도 같이 무시하는 걸로 보임). 만약 태그가 없는 설명 코멘트가 다른 태그들보다 위에 있을 경우 `@desc`를 생략할 수 있다.

파일 코멘트 영역에선 이 태그 대신 `@file`을 사용할 것.

### @param

별칭: `@arg`, `@argument`

함수나 클래스의 파라미터에 대한 설명을 작성한다.

### @returns

별칭: `@return`

함수의 리턴값에 대한 설명.

### @deprecated

```js
/**
 * @deprecated since version 2.0
 */ 
```

### @file

별칭: `@fileoverview`, `@overview`

파일에 대한 설명을 작성하는 태그.

### @class (별칭: `@constructor`)

이 함수는 `new` 키워드로 호출되도록 만들었음을 표시한다. 그러니까 생성자 함수를 반드시 호출해야 하는 경우를 의미한다.

### @classdesc

클래스에 대한 설명을 작성하는 태그.


## 인라인 태그들

### @link

**TODO**

### @tutorial

**TODO**
