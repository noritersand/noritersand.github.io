---
layout: post
date: 2018-03-20 16:45:07 +0900
title: '[JavaScript] 비정형 인자 arguments'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - arguments
---

자바스크립트에는 선언부에 명시되지 않은 파라미터를 별도로 저장하는 프로퍼티가 존재한다.

```js
function argsTest() {
    // 생략
    arguments[2]; // 'arg3'
}

argsTest('arg1', 'arg2', 'arg3');
```

위의 `argsTest()` 호출 시 전달된 세 개의 문자열은 배열로 생성되며 `arguments` 프로퍼티를 통해 접근한다.

```js
function argsTest() {
    if (arguments.length < 1) {
        throw new Error('usage: argsTest(url, 파라미터1, 파라미터2, ...)');
    }
    for (var i = 0; i < arguments.length; i++) {
        if (i != 1 && typeof arguments[i] != 'string') {
            alert('argsTest()의 인수는 문자열만 가능합니다.');
            return;
        }
    }
}
```

요렇게 활용할 수 있다:

```js
const {log} = console;

// doSomething 후에 callback을 호출
function invokeCallbackAfterDoSomthing(callback) {
  // do something 부분
  // 이 영역이 어떤 특정한 함수를 실행하기전에 반드시 실행할 공통 함수 부분
  log(arguments.length);
  var args = [];
  for (var i = 1; i < arguments.length; i++) {
    log('i:', i, 'arguments[i]:', arguments[i])
    args.push(arguments[i]);
  }
  callback.apply(null, args);
}

invokeCallbackAfterDoSomthing(fn, 'PRE', 'AA');

function fn(mode, abc) {
  log('mode:', mode);
  log('abc:', abc);
}
```
