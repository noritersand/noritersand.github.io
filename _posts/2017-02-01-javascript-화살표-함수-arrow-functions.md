---
layout: post
date: 2017-02-01 14:10:00 +0900
title: '[JavaScript] 화살표 함수 Arrow functions'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - arrow-function
  - lambda-function
---

#### 참고한 문서

- [\[MDN\] Arrow function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

#### 버전 정보

- [ES2015에서 최초 정의](https://262.ecma-international.org/6.0/)
- Chrome 45, Edge 12, FireFox 22, Opera 32, Safari 10 이상에서 지원
- IE에서 사용 불가

## 개요

화살표 함수 표현(arrow function expression)에 대한 간단 정리. 익명 함수를 정의하는 간단한 표현식 되겠다.

## 문법

```js
(param1, param2, ..., paramN) => { statements }
(param1, param2, ..., paramN) => expression // 다음과 동일함:  => { return expression; }

// 매개변수가 하나뿐인 경우 괄호는 선택사항:
(singleParam) => { statements }
singleParam => { statements }

// 매개변수가 없는 함수는 괄호 필요:
() => { statements }
```

### `() =>`는 `function()`과 같다

아래 둘은 같다:

```js
var a = () => {};
var a = function() {};
```

얘네도 같다:

```js
var b = (arg) => { console.log(arg); };
var b = function(arg) { console.log(arg); };
```

### `() => ( x )`는 `() => { return x }`와 같다

예를 들어 아래처럼 함수의 모든 표현식이 `return` 문 다음에 있는 경우:

```js
function fn() {
  return 1 + 2;
}
```

이렇게 줄일 수 있다:

```js
// 중괄호가 아니라 소괄호
var fn = () => (1 + 2);
```

### 괄호의 생략

`return` 문 다음의 표현식은 문법이나 연산자 우선순위에 문제가 없을 경우에 한해 괄호를 생략할 수 있다:

```js
var fn = () => 1 + 2;
```

그리고 함수의 매개변수가 딱 하나면 좌변의 괄호도 그냥 빼버릴 수 있음:

```js
var fn2 = str => console.log(str);
fn2('hello'); // 'hello' 출력
```

둘을 합치면 이런 모양이 나온다:

```js
var square = n => n * 2;
console.log(square(2)); // 4 출력
```

## 제한사항

### 생성자 함수로 사용할 수 없음

화살표 함수 표현으로 생성한 함수는 생성자 함수로 호출(`new 키워드`)할 때 에러가 발생한다:

```js
let newbie = () => {
  console.log('noop noop!');
};

newbie(); // noop noop!

new newbie(); // Uncaught TypeError: newbie is not a constructor
```

끗.
