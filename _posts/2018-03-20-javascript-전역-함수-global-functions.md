---
layout: post
date: 2018-03-20 15:40:02 +0900
title: '[JavaScript] 전역 함수 global functions'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - function
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [\[MDN\] JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [http://www.w3schools.com/jsref/jsref_obj_global.asp](http://www.w3schools.com/jsref/jsref_obj_global.asp)


## eval(), uneval()

```
eval( string )
uneval( object )
```

- `string`: 코드로 읽어들일 문자열
- `object`: 문자열로 변환할 객체

문자열 리터럴을 자바스크립트 코드로 읽고 실행하거나 반대로 코드나 객체를 문자열로 변환한다.

예를 몇가지 들면:

- `eval('432 * 10')` 의 경우엔 432 x 10의 결과인 4320이 반환된다.
- `eval('alert()')` 의 경우 경고창이 띄워진다.
- 다음 코드는 `form.search_word`의 value를 반환한다: `eval("document.forms[0].search_word.value");`
- 다음 코드는 name 프로퍼티를 갖는 Javascript Object를 의미한다: `eval('{name="value"}')`

MDN에선 보안 문제가 있고 코드 최적화가 불가능한 `eval()` 대신 [Function 생성자를 이용하라고 권장](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#never_use_eval!)한다:

```js
(
  new Function( 'alert("hi");' )
)();
```

\* `uneval()`은 비표준 함수라서 일부 브라우저에선(구글 크롬) 사용할 수 없다.


## encodeURI(), decodeURI()

```
encodeURI( string )
decodeURI( string )
```

- `string`: 변환할 문자열

문자열을 HTTP 전송에 적합한 코드로 부호화하거나 복호화한다.

```js
encodeURI('가');  // "%EA%B0%80"
decodeURI('%EA%B0%80');  // "가"
```


## encodeURIComponent(), decodeURIComponent()

```
encodeURIComponent( string )
decodeURIComponent( string )
```

- `string`: 변환할 문자열

`encodeURI()`와 `decodeURI()`는 string을 쿼리스트링 전체라고 보며 `?`, `=`, `&`를 변환하지 않는다.
반면 `encodeURIComponent()`와 `decodeURIComponent()`는 string을 쿼리스트링의 일부분이라 보며 예외 없이 모든 문자를 변환한다.

```js
encodeURI('?=&');  // "?=&"
encodeURIComponent('?=&');  // "%3F%3D%26"
```


## isFinite(), isNaN()

```
isFinite( testValue )
```

- `testValue`: 테스트할 수치

수치가 유효(=숫자로 표현 가능한 값인지)한지, 무효한지를 판명한다. 유효수치이면 true 값을 반환하고(return), 무효수치이면 false 값을 반환한다. `isNaN()` 과는 반대 결과를 반환한다.

```
isNaN( testValue )
```

- `testValue`: 테스트할 수치

`isNaN()`은 `isFinite()`의 반대격으로 무효수치이면 true 값을 반환하고(return), 유효수치이면 false 값을 반환한다. (문자열 앞의 공백은 검사할 수 없다.)

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

- `object`: 변환할 수치

값을 부동 소수점 실수로 변환 한다.

```js
parseFloat("10.33"); // 10.33
parseFloat("5.4321e6") // 5432100
```


## parseInt()

```
parseInt( object [, radix ] )
```

- `object`: 변환할 수치.
- `radix`: 기수 (진수 구분)

값을 정수로 변환한다. 숫자로 변환 불가능한 값을 입력받으면 NaN을 반환한다.

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

이 경우 숫자 16진수 100을 10진수로 변환한 결과인 256을 반환한다.

### Number()와 비교

`Number()`는 일부 falsy한 인수(`null`, `false`, `''`)에 대해 0을 반환한다. 반면 `parseInt()`는 `NaN`을 반환한다. 반드시 숫자 타입이 필요한 경우면 `Number()`를 쓰는 게 낫다.

```js
Number(null); // 0
Number(false); // 0
Number(''); // 0
Number(undefined); // NaN

parseInt(null); // NaN
parseInt(false); // NaN
parseInt(''); // NaN
parseInt(undefined); // NaN
```
