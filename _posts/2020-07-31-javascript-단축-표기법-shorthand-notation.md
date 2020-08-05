---
layout: post
date: 2020-08-02 16:25:00 +0900
title: '[JavaScript] 단축 표기법 Shorthand notation'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - shorthand-notation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer](https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)

ES2015에서 사용가능한 프로퍼티와 메서드의 단축 표기 방법을 정리한 글.

## 단축 프로퍼티명

```
{
  variable1, variable2[, variable3, ...]
}
```

이미 정의된 변수가 있을 때, 객체 리터럴에서 그 변수를 그대로 사용한다. 이 때 값의 할당을 생략하면 변수에 담겨진 값이 그대로 할당된다.

```js
var a = 1;
var b = 'two';
var obj = { a, b };
obj; // Object { a: 1, b: "two" }
```

자바스크립트는 pass-by-object-reference이므로, 단축 표기에 사용할 변수의 값이 객체타입일 경우엔:

```js
var a = { b: 1 };
var obj = { a };
obj.a; // Object { b: 1 }

a.b = 2;
obj.a; // Object { b: 2 }
```

이렇게 되므로 주의할 것.

## 단축 메서드명

메서드를 정의할 때 `function` 키워드와 콜론`:`을 생략하는 방식이다.

```
{
  property([parameters]) {},
  * generator() {}
}
```

```js
var obj = {
  // hello: function() { return 'hello world!' }
  hello() { return 'hello world!' }
};
obj.hello(); // "hello world!"
```

generator function을 단축하려면 다음처럼 한다:

```js
var obj = {
  // gen: function* (n) { return 'ssup' }
  * gen() { return 'ssup' }
}
obj.gen().next(); // "ssup"
```
