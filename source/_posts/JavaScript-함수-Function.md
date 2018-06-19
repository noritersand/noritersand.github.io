---
title: 'JavaScript: 함수 Function'
date: 2018-03-20 15:59:09
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - function
---

#### 관련 문서
- http://www.w3schools.com/Js/js_function_definition.asp
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function

## 함수란?
함수란 독립적으로 분리된 로직으로, 미리 정의되어 있거나 사용자 정의에 의해 만들어진 실행가능한 단위를 일컫는 말이다. 자바스크립트에선 함수는 객체로 존재하며 동시에 1급 함수다. 이것은 함수 객체를 변수나 데이터 구조 안에 담거나 전달인자 혹은 반환값으로 사용할 수 있다는 의미다.
```
function 함수이름( [ 매개변수1, 매개변수2..., 매개변수n ] ) { 구문 }
```
함수 선언문은 function 키워드와 함수 이름, 구문 블록의 조합이다. 함수 이름 뒤에는 매개변수를 정의하는데, 매개변수는 타입을 지정하지 않으며 개수 제한이 없다. 매개변수의 타입은 호출될 때 결정된다.

## 함수 선언문(함수 구문)과 함수 정의 표현식
```js
function meIzDaBest() {
  // ...
}
```
함수 선언문은 새 함수 객체를 만들되 함수 이름을 변수로 생성하여 함수 객체를 할당한다. 때문에 함수의 이름이 반드시 필요하며 위 코드와 같은 단 한 가지의 형태만 존재한다.
```js
var a = function() {
  // ...
};

(function() {
  // ...
})();
```
함수 정의 표현식은 말 그대로 함수를 정의하는 표현식이다. '함수 리터럴'이라 하기도 한다. 함수 선언문과 다르게 이름이 필요하지 않아 익명 함수라 부르기도 한다. 위의 코드 중 첫 번째는 함수 정의 표현식으로 생성한 함수를 변수에 할당하는 형태다. 두 번째는 함수를 생성하고 스스로 호출하는 자기 호출 함수(self invoking function)다.

## 함수의 끌어올림(hoisting)
함수 선언문은 코드 중간에 삽입되어 있어도 항상 최상위에서 실행된다. 이것은 함수의 끌어올림(hoisting)이라 부른다. 가령 다음의 경우:
```js
var waaagh = 0; // 변수 선언
function waaagh() { // 같은 이름으로 함수 선언
  // ...
}
waaagh(); // TypeError: waaagh is not a function
waaagh; // 0
```
waaagh라는 식별자로 변수를 정의한 뒤 같은 이름의 식별자에 함수 객체를 할당한 것처럼 보인다. 하지만 실제로는 반대의 순서로 작동한다. 함수가 먼저 변수에 할당되고, 원시 타입값 0이 이를 덮어쓴다.

단, 함수 정의 표현식은 함수 끌어올림이 적용되지 않는다. (하지만 해당 함수 내부의 변수에 대한 끌어올림은 적용된다) 아래를 보면:
```js
fnDeclaration(); // hi
fnExpression(); // TypeError: fnExpression is not a function
function fnDeclaration() {
  console.debug('hi');
}
var fnExpression = function() {
  console.debug('im not');
};
```
fnExpression의 호출문이 표현식보다 먼저 왔을때 타입에러가 발생한다. 왜 참조에러가 아니고 타입에러일까? 위 코드는 실제로 아래처럼 작동하기 때문이다.
```js
function fnDeclaration() {
  console.debug('hi');
}
var fnExpression;
fnDeclaration(); // hi
fnExpression(); // TypeError: fnExpression is not a function
fnExpression = function() {
  console.debug('im not');
};
```
