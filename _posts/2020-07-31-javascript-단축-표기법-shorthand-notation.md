---
layout: post
date: 2020-08-02 16:25:00 +0900
title: '[JavaScript] 단축 표기법 Shorthand notation'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - shorthand-notation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [\[MDN\] Method definitions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)
- [https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer](https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)

#### 버전 정보

- ES2015에서 최초 정의
- Chrome 60/Edge 79/Firefox 55/Opera 47/Safari 11.1 이상에서 사용 가능
- IE에서 사용 불가


## 개요

프로퍼티와 메서드의 단축 표기법을 정리한 글.


## 단축 프로퍼티명

```
{ variable1, variable2[, variable3, ...] }
```

이미 정의된 변수가 있을 때, 객체 리터럴에서 그 변수의 이름 그대로 하면서 값의 할당을 생략한다. 이러면 변수에 담겨진 값이 그대로 할당된다.

```js
var a = 1;
var b = 'two';
var obj = {a, b};

obj; // Object { a: 1, b: "two" }
obj.a === {a}.a; // true
```

그러니까 이 코드는:

```js
{ prop }
```

아래를 생략한 것이다:

```js
{ prop: prop }
```

단축 표기에 사용할 변수의 값이 객체타입일 경우엔:

```js
var a = {b: 1};
var obj = {a};
obj.a; // Object { b: 1 }

a.b = 2;
obj.a; // Object { b: 2 }
```

이렇게 되므로 주의할 것.

### 선언에서 활용

단축 프로퍼티명의 원리를 이용해서 다음처럼 선언하면:

```js
const {log} = console;
```

`log` 상수가 생성되면서 `console.log`가 할당됨.


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
  hello() {
    return 'hello world!';
  }
};
obj.hello(); // "hello world!"
```

generator function을 단축하려면 다음처럼 한다:

```js
var obj = {
  // gen: function* (n) { return 'ssup' }
  * gen() {
    return 'ssup';
  }
}
obj.gen().next(); // Object { value: "ssup", done: true }
```

ES2016부터는 단축 표기법으로 생성한 메서드를 생성자 함수로 사용하면 에러가 발생한다:

```js
var obj = {
  notSure() {}
};
new obj.notSure(); // TypeError: obj.notSure is not a constructor
```

끗.
