---
layout: post
date: 2019-04-21 22:29:00 +0900
title: '[JavaScript] 클래스 Classes'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - class
  - classes
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [\[PoiemaWeb\] 클래스](https://poiemaweb.com/es6-class)
- [\[JAVASCRIPT.INFO\] 클래스와 기본 문법](https://ko.javascript.info/class)

#### 버전 정보

- ES2015에서 최초 정의
- Chrome 72/Edge 79/Firefox 81/Opera 62 이상에서 모든 기능 사용 가능
- IE에서 사용 불가
- Safari 14에서 public static 제외 사용 가능


## 개요

class는 [ES2015](https://www.ecma-international.org/ecma-262/6.0/#sec-class-definitions)에서 소개된 가독성 좋은 새로운 문법이다. 완전히 새로 만들어진 기능이라기 보단 기존 프로토타입 방식을 살짝 개량한 것에 가깝다. (업계에선 이런걸 [Syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)라고 부른다더라)

MDN의 설명에 따르면 class도 사실은 함수라서 사용 방법이 함수처럼 선언문과 표현식으로 나뉜다.


## class 선언

```js
class Newbie {
  constructor(trait) {
    this._trait = trait;
  }
  get trait() {
    return this._trait || 'Know nothing.';
  }
  set trait(arg) {
    this._trait = arg;
  }
  levelUp() {
    console.log('I feel stronger.');
    this._trait = 'Can barely shoot an arrow.';
  }
}

var noob = new Newbie();

noob.trait; // "Know nothing."
noob.levelUp(); // I feel stronger.
noob.trait; // "Can barely shoot an arrow."
noob.trait = 'Can sleep for 12 hours in a row.';
noob.trait; // "Can sleep for 12 hours in a row."
```

함수 선언과 다르게 끌어올림은 발생하지 않는다:

```js
var magicUser = new Sorcerer(); // ReferenceError: can't access lexical declaration `Sorcerer' before initialization

class Sorcerer {}
```


## class 표현식

TODO


## 생성자 constructor

```js
class Numeric extends Number {
  constructor(value) {
    super(value);
  }
}
```

TODO

**생성자 함수(constructor function)가 아님.**


## static

생성자 함수의 메서드를 만드는 키워드.

```js
class Hitman {
  static yo() {
    return 'I kill you!'
  }
}
Hitman.yo();
```


## super

TODO


## public field

```js
class Building {
  location = 'some-where';
  number;

  constructor(number) {
    this.number = number;
  }
}

var building = new Building(1234);
building.location; // "some-where"
building.number; // 1234
```

단순한 인스턴스의 프로퍼티를 생각하면 된다. 메서드 내에서 정의할 땐 `this`를 거쳐야한다.


## private field

```js
class Company {
  #classified = 'actually not';
  #secret;

  constructor(secret) {
    this.#secret = secret;
  }
}

var company = new Company('anyone can see this');
console.log(company); // Object { #classified: "actually not", #secret: "anyone can see this" }

company.#classified; // Uncaught SyntaxError: reference to undeclared private field or method #classified debugger eval code:1:8
company.#secret; // Uncaught SyntaxError: reference to undeclared private field or method #secret
```

private field는 클래스 외부에서 접근하려고 할 때 문법에러가 발생한다. 그렇다고 안보이는 건 아니다.


## 빌트인 프로토타입의 확장

```js
Number.prototype.format = function(n, x) {
  var re = '\\d(?=(\\d{' + (x || 3) + '})+' + (n > 0 ? '\\.' : '$') + ')';
  return this.toFixed(Math.max(0, ~~n)).replace(new RegExp(re, 'g'), '$&,');
}

(123456).format(0); // "123,456"
```

[소스 출처](http://stackoverflow.com/questions/149055/how-can-i-format-numbers-as-money-in-javascript)

위는 Number 프로토타입에 `format` 함수를 추가하는 코드다. 사실 이런식으로 빌트인 프로토타입을 확장하는 것은 추천하지 않는다.

대신 아래처럼 class 문법으로 새로운 프로토타입을 정의한다:

```js
class Numeric extends Number {
  format(n, x) {
    var re = '\\d(?=(\\d{' + (x || 3) + '})+' + (n > 0 ? '\\.' : '$') + ')';
    return this.toFixed(Math.max(0, ~~n)).replace(new RegExp(re, 'g'), '$&,');
  }
}

new Numeric(123456).format(0); // "123,456"
```


## 생성자 함수와의 차이점

### 생성자의 프로퍼티인 `caller`, `arguments`에 접근 불가.

```js
class Test {}
var t = new Test();
t.constructor.caller; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
t.constructor.arguments; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
```


## async method

클래스에 async function을 정의하면 async method가 된다.

TODO

---
