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

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

#### 브라우저 호환

- IE에서 사용 불가

## 개요

화살표 함수 표현(arrow function expression)에 대한 간단 정리.

~~람다 함수가 화살표 함수인가?~~ 이름이야 어쨋든 함수를 정의하는 간단한 표현식 되겠다.

## 문법

```js
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression // 다음과 동일함:  => { return expression; }

// 매개변수가 하나뿐인 경우 괄호는 선택사항:
(singleParam) => { statements }
singleParam => { statements }

// 매개변수가 없는 함수는 괄호 필요:
() => { statements }
```

#### 예시\#1

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

#### 예시\#2

아래처럼 함수의 모든 표현식이 `return` 문 다음에 있는 경우:

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

그냥 괄호를 빼버려도 됨:

```js
var fn = () => 1 + 2;
```

#### 예시\#3

```js
function repeatTwice(fn) {
  return fn() + fn();
}
```

이렇게 함수를 인자로 받는 함수가 정의되어 있다고 하자. 여기서 호출식에 화살표 함수를 적용하면 이렇게 된다:

```js
repeatTwice(() => 'abcd'); // "abcdabcd"
```

`setTimeout()`을 예로 들면:

```js
setTimeout(() => console.log('timeout#2'), 2000); // 2초 후 "timeout#2"

setTimeout(() => {
  console.log('timeout#1');
}, 1000); // 1초 후 "timeout#1"
```

## 제한사항

### 생성자 함수로 사용할 수 없음

화살표 함수 표현으로 생성한 함수는 생성자 함수로 호출(`new 키워드`)할 때 에러가 발생한다:

```js
let newbie = () => {
  console.log('noop noop!');
};

newbie(); // noop noop! debugger eval code:2:11

new newbie(); // Uncaught TypeError: newbie is not a constructor
```

끗.
