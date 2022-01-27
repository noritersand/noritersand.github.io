---
layout: post
date: 2018-03-20 16:24:15 +0900
title: '[JavaScript] 엄격 모드 strict mode'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - strict-mode
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)
- [http://www.w3schools.com/js/js_strict.asp](http://www.w3schools.com/js/js_strict.asp)
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)

#### 테스트 환경

- 파이어 폭스 47
- 크롬 51

## 엄격 모드란?

'strict mode(이하 엄격 모드)'는 ECMAScript 5부터 새로 추가된 기능이다. 스크립트의 시작 부분이나 함수 코드 블록의 시작부분에 `use strict`를 작성하는 것으로 엄격 모드임을 지시한다. 엄격 모드가 아닌 지역은 '표준 모드'로 구분된다. (표준 모드는 'non-strict', 'sloppy mode'라고도 함.)

```js
function test() {
  'use strict';
  // 이 함수는 엄격 모드가 적용됨.
}

(function() {
  'use strict';
  x = 1;
})(); // ReferenceError: assignment to undeclared variable x

// 스크립트 시작 부분에 지시어가 없으면 엄격 모드가 적용되지 않음.
```

`use strict`를 함수 머리에 작성하면 엄격 모드의 적용 범위는 해당 함수로 제한된다. 위 예시의 경우 최상위 스코프는 엄격 모드가 적용되지 않는다.

>표준 모드: 살짝 문제가 있지만... 그냥 넘어가지 뭐.
>
>엄격 모드: 응 에러

엄격 모드의 가장 큰 특징은 잘못된 구문(bad syntax)을 어물쩡 넘어가지 않는다는 것이다. 표준 모드는 비록 잘못된 구문이 있더라도 정말 심각한 오류가 아니라면 알아서 보완하거나 단지 `false`를 반환하는 것으로 그치는데 비해, 엄격 모드에선 잘못된 구문을 항상 에러로 내뱉는다. 에러가 발생했으니 남은 코드의 실행이 중단되는 것은 덤.

## 엄격 모드에서 에러가 발생하는 잘못된 구문들

아래는 표준 모드에서도 문제가 있거나 실패하는 구문이지만 에러가 발생하지는 않던 구문들이다. 엄격 모드에선 에러가 발생한다.

### var로 선언된 변수와 함수 선언문으로 생성된 함수, 함수의 파라미터 삭제 불가

단순히 실패만 하던 삭제 시도는 이제 에러가 발생한다.

```js
function deleteParameter(param) {
  'use strict';
  delete param; // SyntaxError: applying the 'delete' operator to an unqualified name is deprecated
}
deleteParameter;

function deleteFunctionObject(arg) {
  'use strict';
  function aaa() {}
  delete aaa; // SyntaxError: applying the 'delete' operator to an unqualified name is deprecated
}
deleteFunctionObject();

function deleteVariable(arg) {
  'use strict';
  var a = 123;
  delete a; // SyntaxError: applying the 'delete' operator to an unqualified name is deprecated
}
deleteVariable();
```

### non-extensible/non-writable/non-configurable

non-extensible 객체에 프로퍼티를 추가할 때, non-writable 프로퍼티의 값을 변경할 때, non-configurable 프로퍼티를 지우려고 할 때 타입 에러가 발생한다.

```js
function nonextensible() {
  'use strict';
  var obj = {};
  Object.preventExtensions(obj);
  obj.a = 123; // TypeError: can't define property "a": Object is not extensible
}
nonextensible();

function nonwritable() {
  'use strict';
  var obj = {};
  Object.defineProperty(obj, 'a', {
    value: 123,
    writable: false
  });
  obj.a = 456; // TypeError: "a" is read-only
}
nonwritable();

function nonconfigurable() {
  'use strict';
  var obj = {};
  Object.defineProperty(obj, 'a', {
    value: 1,
    configurable: false
  });
  delete obj.a; // TypeError: property "a" is non-configurable and can't be deleted
}
nonconfigurable();
```

### ~~객체 리터럴에 같은 이름의 프로퍼티 중복 불가~~

ES2015에서 허용됨.

### 함수에 동일한 이름의 파라미터가 존재할 수 없음

```js
function strict(param, param) { // SyntaxError: Duplicate parameter name not allowed in this context
  'use strict';
}
```

### getter만 있고 setter가 없는 객체의 프로퍼티에 값 할당 불가

```js
// 'use strict';
var obj = {
  get x() {
    return 0
  }
};
obj.x = 123; // TypeError: setting a property that has only a getter
```

### 원시 타입의 래퍼 객체에 프로퍼티 추가 불가

원시 타입이 변환된 래퍼 객체에 프로퍼티를 추가하거나 기존 프로퍼티의 값을 바꾸는 것은 불가능하며 타입 에러가 발생한다.

```js
'use strict';
false.true = ''; // TypeError: can't assign to properties of (new Boolean(false)): not an object
(14).sailing = 'home'; // TypeError: can't assign to properties of (new Number(14)): not an object
'BBK'.include = 'MB'; // TypeError: can't assign to properties of (new String("BBK")): not an object
```

## 엄격 모드에서 다르게 작동하는 구문들

아래는 표준 모드와 다르게 작동하는 구문들이다. 엄격 모드에서 사용이 금지되거나 허용되지 않는 것도 포함한다.

### 암시적 변수 선언

모든 변수는 선언되지 않고 사용할 수 없다. 표준 모드에선 `var` 키워드 없이 생성된 변수는 마치 전역 변수인 것처럼 자동으로 생성되고 사용할 수 있었지만 엄격 모드에서는 허용되지 않는다.

```js
'use strict';
obj = {}; // ReferenceError: assignment to undeclared variable obj
```

### with문 사용불가

유효범위 체인을 변경하는 `with`문은 엄격 모드에서 사용할 수 없다.

```js
(function() {
  'use strict';
  with (Math) {
    // ...
  }
})(); // SyntaxError: strict mode code may not contain 'with' statements
```

### this의 변화

함수가 메서드가 아닌 함수로 호출될 때 `this`는 `undefined`가 된다(표준 모드에선 함수로 호출될 때의 `this`는 `Window` 같은 전역 객체다). 이를 이용하면 엄격 모드 지원 여부를 판단할 수 있다.

```js
var supportStrictMode = (function() {
  'use strict';
  return this === undefined;
})();
console.debug(supportStrictMode);
```

함수가 `call()`이나 `apply()`로 호출될 때 this의 값은 호출표현식의 첫 번째 인자값이다. 만약 첫 번째 인자가 원시 타입이면 표준 모드에선 래퍼 객체로 받아오지만, 엄격 모드에선 원시 타입 그대로 받아온다.

```js
function standard(arg) {
  console.debug(this); // Number { 123 }
  console.debug(typeof this); // object
}
standard.call(123);

function strict(arg) {
  'use strict';
  console.debug(this); // 123
  console.debug(typeof this); // number
}
strict.call(123);
```

### eval() 함수의 유효범위

엄격 모드에서 `eval()` 함수는 함수 안의 중첩된 유효범위로 취급된다. 때문에 `eval()`의 전달인자에 의해 생성된 변수나 함수가 있다면 이것은 `eval()` 내에서만 존재하게 된다.

```js
'use strict';
eval('function a() { console.debug(123); }');
a(); // ReferenceError: a is not defined
```

### arguments 객체의 분리

표준 모드에선 함수의 매개변수와 `arguments`의 원소가 같은 참조값을 갖는다. 엄격 모드에선 이 둘이 분리된다.

```js
function standard(param) {
  param = 1;
  console.log(arguments[0]); // 1
}
standard(123);

function strict(param) {
  'use strict';
  param = 1;
  console.log(arguments[0]); // 123
}
strict(123);
```

### 8진수 리터럴 사용 불가

```js
'use strict';
console.debug(010); // SyntaxError: octal literals and octal escape sequences are deprecated
```

### 함수의 프로퍼티인 arguments, caller, callee에 접근 불가

```js
'use strict';
function fn() {
  'use strict';
  return arguments.callee;
};
fn(); // TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
fn.caller; // TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
fn.arguments; // TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
```

### eval과 arguments는 키워드로 취급됨

키워드는 식별자로 사용할 수 없음.

```js
'use strict';
var eval = 123; // SyntaxError: redefining eval is deprecated
var arguments = 123; // SyntaxError: redefining arguments is deprecated
```

### 엄격 모드에서 사용할 수 없는 예약어들

다음 목록은 키워드로 예약되어 있어서 식별자로 사용할 수 없다. (변수, 함수, 파라미터, 레이블의 이름으로 사용 불가):

- implements
- interface
- let
- package
- private
- protected
- public
- static
- yield
