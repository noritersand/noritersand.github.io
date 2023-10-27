---
layout: post
date: 2023-08-08 18:39:19 +0900
title: '[JavaScript] new.target'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - new.target
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target)

#### 브라우저 호환

- IE 제외 모두 지원(2023-08-08)


## 개요

메타 속성인 `new.target`은 함수가 함수로 호출되었는지, 아니면 생성자로 호출되었는지를 구분할 때 사용한다. ES2015에 추가된 기능이다.


## 기본

`new.target`은 함수 내에서만 사용할 수 있다:

```js
new.target; // Uncaught ReferenceError: target is not defined
```

함수로 호출된 경우 `undefined`를, 생성자일 땐 `new`로 호출된 생성자 또는 함수에 대한 참조를 반환한다:

```js
function Foo() {
    return new.target;
}

Foo(); // undefined
new Foo(); // function Foo()
new Foo().name; // 'Foo'
```

이런 식으로 활용한다:

```js
function Newbie(name) {
  if (!new.target) {
    return new Newbie();
  }
  this.name = name;
}

var noob = new Newbie('noob-noob');
var noob2 = Newbie('noob-noob');
noob instanceof Newbie; // true
noob2 instanceof Newbie; // true 
```


## 추가 정보

문법에 따라 미묘한 차이가 있다. 클래스(classes)는 클래스를 반환하거나, `super()`가 호출된 경우 서브클래스를 반환한다:

```js
class A {
  constructor() {
    console.log(new.target.name);
  }
}

class B extends A {
  constructor() {
    super();
  }
}

var a = new A(); // logs "A"
var b = new B(); // logs "B"

// 코드 출처: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new.target
```

화살표 함수는 `new.target`을 둘러싸는 함수의 `new.target`을 반환한다:

```js
function Bar() {
    return () => new target
}

new Bar(); // function Bar()
```


## 꼐속...
