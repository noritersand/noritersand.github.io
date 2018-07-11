---
title: 'JavaScript: 비정형 인자 arguments'
date: 2018-03-20 16:45:07
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - arguments
---

자바스크립트에는 선언부에 명시되지 않은 파라미터를 별도로 저장하는 프로퍼티가 존재한다.
```js
function argsTest() {
    // ... 생략
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
