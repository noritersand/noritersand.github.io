---
layout: post
date: 2018-03-20 15:57:09 +0900
title: '[JavaScript] 변수의 유효범위와 끌어올림 hoisting'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - hoisting
---

* Kramdown table of contents
{:toc .toc}


## 함수 단위의 유효범위

C, JAVA같은 프로그래밍 언어는 블록 안에 있는 코드는 자신만의 유효범위를 가지며, 변수는 해당 변수가 선언되지 않은 블록 밖에서는 보이지 않는다. 이를 블록 단위의 유효범위라 부르는데 자바스크립트는 이와 달리 함수 단위의 유효범위를 갖는다. 함수 단위 유효범위란 함수 안에서 선언된 모든 변수가 함수 내 전체에서 유효하다는 의미다.

```js
function fn() {
  {
    var scope = 'local'; // 블록 내에서 선언.
  }
  return scope; // 같은 함수 내에 있으므로 접근 가능
}
fn(); // local
```

다른 언어에 익숙하다면 위 코드가 말이 안되는 것처럼 보일 수 있다. 하지만 '함수 단위의 유효범위'의 의미를 생각해보면 충분히 말이 된다. 자바스크립트는 이런식으로 scope를 블록이 아닌 함수로 구분한다.


## 끌어올림, hoisting

자바스크립트 코드는 함수 안에 있는 모든 변수를 함수 맨 꼭대기로 '끌어올린'것처럼 작동한다. 다만 이 '끌어올린'다는 표현은 변수의 선언에만 해당하는 것으로 값을 할당하는 변수의 초기화는 제외한다. 가령 다음을 보면:

```js
var scope = 'global';
function a() {
  console.log(scope); // undefined
  var scope = 'local';
  console.log(scope); // local
}
a();
```

3행에서 지역 변수 scope는 아직 선언되지 않았으므로 전역 변수 scope의 값인 "global"을 출력해야 한다고 오해할 수 있다. 하지만 "undefined"가 출력되는데 위 코드는 실제로 아래와 같이 작동하기 때문이다:

```js
var scope = 'global';
function a() {
  var scope;
  console.log(scope); // undefined
  scope = 'local';
  console.log(scope); // local
}
a();
```

변수는 그 변수가 정의된 함수의 시작부분에서 생성되지만 var 구문이 실행되기 전까지는 초기화되지 않는다.. 즉, 변수는 존재하지만 초기화 되기 전에 참조했으므로 undefined가 반환되는 것이다.

```js
console.log(grrr); // can't access lexical declaration `grrr' before initialization
let grrr = 123;
```

예외로 let 키워드로 선언된 변수의 경우 끌어올려지긴 하지만 초기화 전에 호출했을 때 참조 에러가 발생한다.

끌어올림은 변수뿐만 아니라 함수에도 적용된다. 요 링크를 보자: [\[이 블로그 내부 링크\] 함수](/javascript/javascript-함수-function/)
