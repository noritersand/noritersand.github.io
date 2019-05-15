---
layout: post
date: 2017-02-01 14:10:00 +0900
title: 'JavaScript: 화살표 함수 arrow functions'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - arrow function
  - lambda function
---

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)

#### 브라우저 호환

- IE에서 사용 불가

## 화살표 함수

화살표 함수가 람다 함수겠지? 이름이야 어쨋든 익명 함수를 구현하는 간단한 표현식 되겠다.

```js
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression // 다음과 동일함:  => { return expression; }

// 매개변수가 하나뿐인 경우 괄호는 선택사항:
(singleParam) => { statements }
singleParam => { statements }

// 매개변수가 없는 함수는 괄호 필요:
() => { statements }
```

아아아아아아주 간단한 예시#1:

```js
// 아래 둘은 같다.
var a = () => {};
var a = function() {};

// 얘네도 같다.
var b = (arg) => { console.log(arg); };
var b = function(arg) { console.log(arg); };
```

아아아아아아주 간단한 예시#2:

```js
function repeatTwice(fn) {
  return fn() + fn();
}
repeatTwice(() => 'abcd'); // "abcdabcd"
```

`setTimeout()` 함수를 예로 들면 다음과 같다:

```js
setTimeout(() => console.log('timeout#2'), 2000); // 2초 후 "timeout#2"

setTimeout(() => {
  console.log('timeout#1');
}, 1000); // 1초 후 "timeout#1"
```
