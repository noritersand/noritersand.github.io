---
title: 'JavaScript: 타입 변환 type conversion'
date: 2018-03-20 16:15:11 +09:00
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - type
  - conversion
---

#### 참고한 글
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)

## 암시적 타입 변환
자바스크립트는 필요에 따라 타입 변환을 자동으로 수행한다. 가령 문자열이 필요한 위치에 할당된 값이 문자열이 아니면 문자열 타입으로 변환을 시도할 것이다. 이 과정에서 타입이 올바르지 않아 에러가 발생하는 경우는 거의 없다. 다음은 암시적 타입 변환의 사례들이다:
```js
if (1) {
  console.log('1 is true');
}
if (!'') {
  console.log('null string is false');
}
if ('false') {
  console.log('"false" is true');
}
```
if문의 조건으로 불리언이 아닌 값이 할당되면 이 값은 불리언 타입으로 변환된다.
```js
var a = 1;
a = a + '';
typeof a; // "string"
typeof +a; // "number"

var b = "2";
typeof b++; // "number"

var c = "xxx";
-c; // NaN
c++; // NaN
```
산술 연산자 `+`는 피연산자가 문자열일 때 다른 피연산자를 문자열로 변환한다. `+`가 단항 연산자로 사용되면 피연산자를 숫자로 변환한다. 단항 연산자 `-`와 증감 연사자 `--`, `++`의 암시적 변환도 동일하게 작동한다. 만약 숫자로 변환할 수 없는 문자열이면 NaN으로 변환된다.
```js
1 == "1"; // "1"은 1로 변환
true == 1; // true는 1로 변환
false == 0; // false는 0으로 변환
"1" == true; // "1"은 true로 변환
['a'] == 'a'; // 객체는 toString()이나 valueOf()를 사용해 문자열로 변환
```
동등 연산자 `==`는 피연산자들이 서로 타입이 다를 때 타입 변환을 시도한다.
규칙은 다음과 같다:
- 둘 중 하나가 숫자이고 나머지 하나가 문자열일 때 문자열을 숫자로 변환한다.
- 둘 중 하나가 불리언이고 나머지 하나가 숫자일 때 불리언을 숫자로 변환한다.
- 둘 중 하나가 문자열이고 나머지 하나가 불리언일 때 문자열을 불리언으로 변환한다.
- 둘 중 하나가 객체고 나머지 하나가 숫자 또는 문자열이면 객체를 문자열로 변환한다. 이 때 해당 객체의 `toString()`이나 `valueOf()`가 사용된다.

## 명시적 타입 변환
```js
Boolean('abcd'); // true
String(123); // "123"
String(true); // "true"
Number('123'); // 123
Number('a123'); // NaN
Object(123); // new Number(123)과 같다.
```
자동으로 수행되는 타입 변환 말고도 명시적인 타입 변환이 필요할 수 있다. 원시 타입으로의 변환을 원할 경우엔 단순히 `Boolean()`, `Number()`, `String()`, `Object()` 함수를 사용하면 된다.
```js
Boolean({ a: 1 }); // true, 객체는 모두 true로 변환된다.
```
객체를 불리언으로 변환할 때는 단 하나의 규칙만 기억하면 된다: 객체는 무조건 true다.
```js
var obj = { a: 1, b: 2 };
String(obj); // [object Object]

var arr = [1, 2, 3];
String(arr); // "1,2,3"

function fn() { console.log('do something'); }
String(fn); // "function fn() { console.log('do something');}"

String(/D/g); // ""/D/g"
String(new Date()); // "Thu Jul 07 2016 15:32:36 GMT+0900"
```
객체를 문자열로 변환하려고 할 때 객체가 `toString()` 메서드를 갖고 있다면 자바스크립트는 이 메서드를 호출할 것이다. 만약 `toString()`을 갖고 있지 않거나 원시 타입을 반환하지 않으면 `toString()` 대신 `valueOf()`를 찾는다. `valueOf()`가 원시 타입을 반환하면 이 값을 문자열로 변환하겠지만 만약 `valueOf()`가 없거나 원시 타입을 반환하지 않으면 TypeError가 발생한다.
```js
Number([]); // 0
Number([1]); // 1
Number([1, 2]); // NaN
```
객체를 숫자로 변환할 땐 문자열과 반대로 `valueOf()`를 먼저 찾는다. `valueOf()`가 있고 원시 타입을 반환하면 이 값을 숫자로 변환하여 돌려준다. `valueOf()`가 없거나 원시 타입을 반환하지 않으면 자바스크립트는 `toString()`을 찾으며 같은 작업을 수행한다. 만약 `toString()`이 없거나 원시 타입을 반환하지 않으면 TypeError가 발생할 것이다.
```js
"abcdef".valueOf(); // "abcdef"
/D/g.valueOf(); // /D/g
new Date().valueOf(); // 1467874589125, 1970-01-01 부터 지난 시간을 밀리초로 표현한 값
```
`valueOf()` 메서드는 오직 래퍼 객체와 정규식, Date 타입에서만 의미 있다. 기본적으로 `valueOf()`는 원시 타입으로 변환된 값을 돌려주도록 되어 있는데 대부분의 객체가 원시 타입으로 변환될 수 없으므로 단지 자기 자신(객체 자체)을 돌려줄 뿐이다. 빈 배열이 숫자 0으로 변환되는 것은 이런 특성탓이라 설명할 수 있다.
Array 타입은 `valueOf()` 대신 `toString()`을 호출한다(규칙에 따라 `valueOf()`를 먼저 찾았으나 원시 타입이 아닌 객체를 돌려주므로 `toString()`을 대신 호출하는 것). `toString()`은 빈 배열을 빈 문자열로 돌려주며 빈 문자열을 숫자로 바꾸면 0이 되는 것이다.

## 자바스크립트 타입 변환 표
출처: JavasScript: The Definitive Guide. David Flanagan 저. 6판. 58쪽

|                  | String      | Number | Boolean | Object                 |
|------------------|-------------|--------|---------|------------------------|
| undefined        | "undefined" | NaN    | false   | TypeError              |
| null             | "null"      | 0      | false   | TypeError              |
| true             | "true"      | 1      |         | new Boolean(true)      |
| false            | "false"     | 0      |         | new Boolean(false)     |
| "" (null-string) |             | 0      | false   | new String("")         |
| "1.2"            |             | 1.2    | true    | new String("1.2")      |
| "one"            |             | NaN    | true    | new String("one")      |
| " "              |             | 0      | true    | new String(" ")        |
| 0                | "0"         |        | false   | new Number(0)          |
| -0               | "0"         |        | false   | new Number(-0)         |
| NaN              | "NaN"       |        | false   | new Number(NaN)        |
| Infinity         | "Infinity"  |        | true    | new Number(Infinity)   |
| -Infinity        | "-Infinity" |        | true    | new Number(-infinity)  |
| 123              | "123"       |        | true    | new Number(123)        |
| { }              |             |        | true    |                        |
| [ ]              |             | 0      | true    |                        |
| [9]              | "9"         | 9      | true    |                        |
| ["a"]            |             | NaN    | true    |                        |
| [1, 2]           |             | NaN    | true    |                        |
| function() { }   |             | NaN    | true    |                        |
