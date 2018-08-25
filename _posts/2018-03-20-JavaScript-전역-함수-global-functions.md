---
layout: post
date: 2018-03-20 15:40:02 +0900
title: 'JavaScript: 전역 함수 global functions'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - function
---

#### 참고한 글
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [http://www.w3schools.com/jsref/jsref_obj_global.asp](http://www.w3schools.com/jsref/jsref_obj_global.asp)

## eval(), uneval()
```
eval( jsString )
uneval( object )
```
- **jsString**: 코드로 읽어들일 문자열
- **object**: 문자열로 변환할 객체

문자열 리터럴을 자바스크립트 코드로 읽고 실행하거나 반대로 코드나 객체를 문자열로 변환한다.

예를 몇가지 들면:
- eval('432 * 10') 의 경우엔 432 x 10의 결과인 4320이 리턴된다.
- eval('alert()') 의 경우 경고창이 띄워진다.
- 다음 코드는 form.search_word의 value를 리턴한다: `eval("document.forms[0].search_word.value");`
- 다음 코드는 name프로퍼티를 갖는 Javascript Object를 의미한다: `eval('{name="value"}')`

참고로 eval()을 대신하여 Function 생성자를 이용하기도 한다.
```js
( new Function( 'alert("hi");' ) )();
```
uneval()은 비표준 함수로 chrome에서 지원되지 않는다.

## encodeURI(), decodeURI()
```
encodeURI( jsString )
decodeURI( jsString )
```
- **jsString**: 변환할 문자열

문자열을 HTTP 전송에 적합한 코드로 암호화하거나 복호화한다.
```js
encodeURI('가');  // "%EA%B0%80"
decodeURI('%EA%B0%80');  // "가"
```


## encodeURIComponent(), decodeURIComponent()
```
encodeURIComponent( jsString )
decodeURIComponent( jsString )
```
- **jsString**: 변환할 문자열

encodeURI()는 jsString을 쿼리스트링의 전체라고 보며 "?", "=", "&"를 변환하지 않는다.
반면 encodeURIComponent()는 jsString을 쿼리스트링의 일부분이라 보며 예외 없이 모든 문자를 변환한다.
```js
encodeURI('?=&');  // "?=&"
encodeURIComponent('?=&');  // "%3F%3D%26"
```


## isFinite(), isNaN()
```
isFinite( testValue )
```
- **testValue**: 테스트할 수치

수치가 무효 수치인가, 유효한 수치인가를 판명한다. 유효수치이면 true 값을 반환하고(return), 무효수치이면 false 값을 반환한다. isNaN() 과는 반대 결과를 반환한다.
```
isNaN( testValue )
```
- **testValue**: 테스트할 수치

isNaN()은 isFinite()의 반대격으로 무효수치이면 true 값을 반환하고(return), 유효수치이면 false 값을 반환한다. (문자열 앞의 공백은 검사할 수 없다.)
```js
isFinite(2002);  // true
isFinite('123.4567');  // true
isFinite('a');  // false

isNaN('123.4567');  // false
isNaN(1);  // false
isNaN("Hello");  // true
```

## parseFloat()
```
parseFloat( object )
```
- **object**: 변환할 수치

값을 부동 소수점 실수로 변환 한다.
```js
parseFloat("10.33"); // 10.33
parseFloat("5.4321e6") // 5432100
```

## parseInt()
```
parseInt( object [, radix ] )
```
- **object**: 변환할 수치.
- **radix**: 기수 (진수 구분)

값을 정수로 변환한다. 숫자로 변환 불가능한 값을 입력받으면 NaN을 리턴한다.
```js
parseInt(100); // 100
parseInt('100'); // 100
parseInt(new String(1100)); // 1100
parseInt('10.00'); // 10
parseInt('10.98'); // 10
parseInt('40 years'); // 40
parseInt('010'); // 10
parseInt('0x10'); // 16
```
radix가 있고 없고의 결과가 다르다. 예를 들어 radix가 16일 때 object는 16진수로 간주된다.
```js
parseInt(100, 16); // 256
```
이 경우 숫자 16진수 100을 10진수로 변환한 결과인 256을 리턴한다.
