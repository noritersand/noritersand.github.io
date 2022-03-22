---
layout: post
date: 2018-03-20 14:57:36 +0900
title: '[JavaScript] 연산자 Operator'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - operator
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] 표현식과 연산자 Expressions and operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)
- [\[MDN\] 동치 비교 및 동일성 Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)

## 산술 연산

```
+   -   *   /   %   ++   --
```

덧셈 연산자는 피연산자 중 하나가 문자열일 때 나머지 피연산자도 문자열로 변환하려고 시도한다. 그래서 `'nan' + 1`의 결과는 'nan1'이 된다.
증가/증감`++ --` 연산자는 피연산자를 가능할 경우 숫자 타입으로 변경한다. 가령 `x++`에서 `x`가 문자열 `"1"`이면 `x`는 숫자 1로 바뀌며 이후 1씩 증가된다. `x`가 숫자로 바꿀 수 없는 문자열일 경우 `NaN`을 반환한다.

## 할당 연산

```
=    +=    -=    *=    /=    %=
&=    ^=    |=    <<=    >>=    >>>=
```

## 비교 연산

```
==    ===    !=    !==    >    <    >=    <=
```

`===`는 일치 연산자 혹은 엄격한 동등 연산자라고 한다. `==`는 동등(equal)을 판단하며 `===`는 일치(identical)하는지 판단한다. 둘은 같음을 정의하는 기준이 다르다. 가령 `===`는 값이 같아도(형변환을 통해 같다고 판단되는 값. 가령 1과 '1'은 동등하다.) 타입이 다를 경우 둘이 서로 다른것으로 판단한다.

동등 연산`==`의 판단 기준:
- 두 값의 타입이 같은 경우, 두 값이 일치하면 둘은 동등하다.
- 두 값의 타입이 다른 경우에도 여전히 동등할 여지가 있으므로 타입 변환이 사용된다.
- 두 값 중 하나가 `null`이고 다른 하나가 `undefined`라면 두 값은 동등하다.
- 두 값 중 하나가 문자열이고 다른 하나가 숫자면 문자열을 숫자로 변환한다.
- 두 값 중 하나가 `true`이면 이를 1로 변환, `false`일 땐 0으로 변환 후 비교한다.
- 한 값이 객체고 다른 하나가 숫자 또는 문자열이면 객체를 원시 타입으로 변환 후 비교한다. 객체의 원시 타입 변환에는 해당 객체의 `toString()` 메서드나 `valueOf()` 메서드가 사용된다.

일치 연산`===`의 판단 기준:
- 두 값의 타입이 서로 다르면 두 값은 일치하지 않는다.
- 두 값이 모두 `null`이거나 `undefined`면, 두 값은 일치한다.
- 두 값이 모두 불리언 값 `true`이거나 `false`일 경우에 두 값은 일치한다.
- 적어도 하나의 값이 `NaN`이면 두 값은 일치하지 않는다. `NaN` 값은 자기 자신을 포함해 다른 어떠한 값과도 일치하지 않는다. 임의의 값 x가 `NaN`인지 검사하기 위해서는 `x !== x`와 같이 사용한다. 앞의 표현식이 참을 만족 할 때만 x의 값이 `NaN`이 된다.
- 두 값이 모두 숫자고 같은 값을 갖는다면, 두 값은 일치한다. 만약 하나의 값이 0이고 다른 하나의 값이 -0일지라도, 두 값은 일치한다.
- 두 값이 모두 문자열이고, 같은 위치에 정확히 같은 16비트 문자열 값을 갖고 있다면, 두 값은 일치한다. 만약 문자열의 길이나 내용이 다를 경우, 두 값은 일치하지 않는다. 두 문자열이 같은 의미를 갖고, 육안상 같은 문자열을 갖더라도 16비트 값의 순서가 다르게 인코딩되어 있을 수도 있다.
- 자바스크립트에서는 유니코드 문자열에 대해서 정규화 과정을 수행하지 않는다. 또한 `===`나 `==` 연산자를 사용해 유니코드 문자열의 동등 비교를 할 수 없다. 이와 같은 문자열을 비교하려면 `String.localeCompare()`를 사용한다.
- 두 값이 모두 같은 객체나 배열 또는 함수를 참조하고 있으면, 두 값은 일치한다. 두 값이 서로 다른 객체를 참조할 경우에 설사 두 객체의 프로퍼티가 일치하더라도 두 값은 일치하지 않는다.

## 논리 연산

```js
&&    ||    !
```

```js
var a = 0, b = 1;
a == 0 && b == 1; // true
true && b == 0; // false
a != b || b != b; // true
false || true; // true
false || (true && false); // false
```

논리 연산자 `&&`, `||` 는 피연산자가 관계 표현식 또는 `boolean` 타입이거나 괄호로 감싼 논리 표현식일 땐 일반적인 AND와 OR연산을 수행한다.
만약 피연산자가 `boolean`이 아닐 때 해당 피연산자는 `boolean` 타입으로 변환된다. 이후 규칙에 따라 값을 반환하는데, 반환되는 값은 `boolean`이 아니라 `boolean`으로 변환되기 전인 원래의 값이 반환된다. 가령 다음을 보면:

```js
undefined && 1; // undefined
```

undefined가 `false`로 변환되고 연산식을 평가한 후 다시 원래의 값인 `undefined`가 반환된다.

```js
1 && 2; // 2
3 && 1; // 1
0 && undefined; // 0
undefined && 0; // undefined
123 && null; // null
null && 123; // null
```

`&&`는 다음 규칙을 따른다:
- 피연산자가 모두 `true`일 땐 우변 피연산자의 값을 반환
- 피연산자가 모두 `false`일 땐 좌변 피연산자의 값을 반환
- 둘 중 하나가 `false`일 땐 `false`로 평가되는 값을 반환

```js
[] || {}; // Array [  ]
"123" || window; // "123"
null || undefined; // undefined
undefined || null; // null
0 || 1; // 1
1 || 0; // 1
```

`||`는 다음 규칙을 따른다:
- 피연산자가 모두 `true`일 땐 좌변 피연산자의 값을 반환
- 피연산자가 모두 `false`일 땐 우변 피연산자의 값을 반환
- 둘 중 하나가 `false`일 땐 `true`로 평가되는 값을 반환

다른 연산자도 마찬가지지만 논리 연산자 또한 피연산자를 비교하기 전에 각 피연산자를 평가(evaluate)하는 과정을 거친다. 표현식이나 호출식을 먼저 실행한 후 비교한다고 생각하면 된다. 그리고 상황에 따라 우변의 피연산자는 평가될 수도 있고 평가되지 않을 수도 있다는 것을 주의해야 한다. 다음 예를 보면:

```js
alert(1) || alert(2);
```

OR 연산은 좌변이 `false`일 때 우변도 평가한다. (`window.alert`의 반환값은 `undefined`) 따라서 경고창은 두 번 나타난다.

```js
alert(1) && alert(2);
```

AND 연산은 좌변이 `false`일 때 우변을 평가하지 않는다. 따라서 경고창은 한 번 나타난다.

```js
true || alert(2);
```

OR 연산은 좌변이 `true`일 때 우변을 평가하지 않는다. 이 경우 경고창이 나타나지 않는다.

```js
true && alert(2);
```

AND 연산은 좌변이 `true`일 때 우변을 평가한다. 경고창이 나타난다.

다음은 논리 연산자로 삼항 연산을 흉내낸 것이다:

```js
var a = 1;
var result = (a == 1) && 'equal' || 'not equal';
console.log(result); // "equal"
```

OR 연산자의 좌변과 우변이 모두 `true`로 평가되는 값일 때만 제대로 작동하는 한계가 있지만...

## 비트 연산

- `~`: 비트단위 NOT 연산
- `&`: 비트단위 AND 연산
- `|`: 비트단위 OR 연산
- `^`: 비트단위 XOR 연산
- `<<`: left shift operator, 왼쪽으로 이동하며 새로운 자리는 0으로 채운다.
- `>>`: right shift operator, 부호 비트를 확장(유지)하면서 오른쪽으로 이동하며 새로운 자리는 0으로 채운다.
- `>>>`: unsigned right shift operator, 부호 비트 확장 없이 오른쪽으로 이동. `>>`와 다르게 부호 비트를 무시하기 때문에 좌변의 피연산자가 음수일 경우 결과에서 양수로 바뀐다.

## 삼항 연산자 Conditional (ternary) operator `?`

조건 연산자 또는 선택 연산자. TRUE 혹은 FALSE에 해당하는 값을 반환한다.

```
조건 ? TRUE : FALSE
```

```js
var a = 1;
var result = (a == 1) ? "일" : "일 아님";
console.log(result); // "일"
```

## instanceof

객체의 프로토타입 체인에 생성자의 프로토타입이 존재하는지 테스트한다. 좌변의 피연산자로 객체를 받고 우변의 피연산자로 생성자를 받는다. 이 연산자로 객체가 특정 타입의 인스턴스인지 판단할 수 있다. 우변은 반드시 함수 이름이어야 하는데, 만약 함수가 아니면 TypeError가 발생할 것이다.

```
object instanceof constructor
```

```js
function fn() {
    // blah blah blah
}

fn instanceof Function;  // true
alert instanceof Function;  // true
"" instanceof String; // false, 문자열 자체는 원시 타입이기 때문
new String() instanceof String;  // true, String 래퍼 객체는 String의 인스턴스
[] instanceof Array; // true, [].__proto__ === Array.prototype
[] instanceof Object; // true, 모든 객체는 Object의 인스턴스
```

## typeof

연산자 다음에오는 변수, 함수, 객체 또는 표현식의 타입을 반환한다.

```
typeof expression
```

```js
typeof "John"                // "string"
typeof 3.14                   // "number"
typeof NaN                    // "number"
typeof false                  // "boolean"
typeof [ 1, 2, 3, 4 ]           // "object"
typeof { name: 'John', age: 34 }  // "object"
typeof new Date()             // "object"
typeof function() {}         // "function"
typeof myCar                  // "undefined" (if myCar is not declared)
typeof null                   // "object"
```

## in

객체나 배열에 특정 프로퍼티가 존재하는지 확인한다.

```
expression in Object
```

```js
// Arrays
var cars = [ "Saab", "Volvo", "BMW" ];
"Saab" in cars          // false, 'Saab'은 프로퍼티의 이름이 아니라 값이므로 false
0 in cars               // true
1 in cars               // true
4 in cars               // false, cars의 길이는 3이므로 배열의 4번째 인덱스는 없다.
"length" in cars        // true, Array 래퍼 객체에 length란 프로퍼티가 존재함.

// Objects
var person = { firstName: "John", lastName: "Doe", age: 50 };
"firstName" in person   // true
"age" in person         // true

// Predefined objects
"PI" in Math            // true
"NaN" in Number         // true
"length" in String      // true
```

또한 in은 for-in 반복문에서 사용된다.

```
for (variable in object) { }
```

- `object`: 반복할 객체. 객체의 프로퍼티 만큼 반복된다.
- `variable`: 매번 반복될 때마다 프로퍼티의 key를 할당한다. 만약 반복되는 객체가 배열일 경우 인덱스를 할당한다.

```js
var foo = { a: 1, b: 2};
var propNames = [];
for (var ele in foo) {
   propNames.push(ele);
}
console.log(propNames); // [ "a", "b" ]

var foo2 = [ 'aaa', 'bbb', 'ccc' ];
var idxs = [];
for (var ele in foo2) {
  idxs.push(ele);
}
console.log(idxs); // [ "0", "1", "2" ]
```

## delete

프로퍼티를 삭제한다. 삭제에 성공했을 때 `true`를, 실패했을 때 `false`를 반환한다.

```
delete Object.property
```

```js
var fn = { word: "hi" };

console.log(fn.word);   // "hi"

delete fn.word; // true
console.log(fn.word); // undefined
```

`var`문으로 정의된 변수(전역 객체의 프로퍼티라 하더라도), 함수 선언문(또는 함수 구문)으로 정의된 함수, 그리고 함수의 매개변수는 삭제할 수 없다.

```js
var a = 1;
delete a; // false

function fn() { }
delete fn; // false

function callee(callback) {
  console.debug(delete callback); // false
}
callee(function() {});
```

## void

```
void expression
```

`void` 연산자는 피연산자를 실행하고 `undefined`를 반환한다. 피연산자를 실행해야 하지만 결과를 노출하고 싶지 않을때 사용한다.

```html
<a href="javascript:void window.open()">open</a>
```

`void`는 같은 이름의 함수가 존재하는데:

```js
void(0); // undefined
void(); // SyntaxError: expected expression, got ')'
```

연산자와 마찬가지로 항상 `undefined`를 반환하는 함수다. 전달인자는 어느 값이어도 상관 없지만 할당하지 않으면 에러가 발생한다. 이동할 주소가 없는 앵커 태그가 필요할 때 종종 사용된다.

```html
<a href="javascript:void(0)">test</a>
```

## 지수 연산자 [Exponentiation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation) `**`

첫 번째 피연산자를 두 번째 피연산자로 거듭제곱한 결과를 반환한다.

```js
2 ** 8 // 256
2 ** 32 // 4294967296
10 ** 1 // 10
10 ** 0 // 1
10 ** -1 // 0.1
10 ** -2 // 0.01
```

`**` 연산자는 오른쪽으로 결합(오른쪽의 연산자가 우선권을 가짐)된다. 따라서 `a ** b ** c`는 `a ** (b ** c)`와 같다.

## Optional Chaining `?.`

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
- [https://tc39.es/proposal-optional-chaining/#top](https://tc39.es/proposal-optional-chaining/#top)
- [https://javascript.info/optional-chaining](https://javascript.info/optional-chaining)

다른 언어에 Elvis Operator`?:`라는 뇨솤이 있는데 이와 비슷한 기능이 추가되었다. 대신 자바스크립트에선 `?.`라고 쓴다.

```
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
```

객체 접근 연산자 `.`를 대체해 사용할 수 있다. 대상 객체가 `undefined`여도 에러 대신 `undefined`를 반환한다.

2022-03-02 아직 드래프트 상태인 것 같은데 어째선지 모든 브라우저에서 다 된다.

TODO

## 널 병합 연산자 Nullish coalescing operator `??`

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)

```
leftExpr ?? rightExpr
```

`A || B` 대신 사용할 수 있는 연산자. 연산자 좌변이 `null` 혹은 `undefined`로 평가되면 연산자 우변의 결과를 반환한다.

TODO
