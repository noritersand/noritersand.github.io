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

#### 참고 문서

- [Classes \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [클래스 \| PoiemaWeb](https://poiemaweb.com/es6-class)
- [클래스와 기본 문법 \| JAVASCRIPT.INFO](https://ko.javascript.info/class)

#### 테스트 환경 정보

- ES2015에서 최초 정의
- Chrome 72/Edge 79/Firefox 81/Opera 62 이상에서 모든 기능 사용 가능
- IE에서 사용 불가
- Safari 14에서 public static 제외 사용 가능


## 개요

class는 [ES2015](https://www.ecma-international.org/ecma-262/6.0/#sec-class-definitions)에서 소개된 가독성을 위해 만들어진 문법이다. 사실 완전히 새로운 문법이라기 보단 기존 프로토타입 방식을 살짝 개량한 것에 가깝다. 업계에선 이런걸 [Syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)라고 부른다고 함.

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

**TODO**


## 생성자 constructor

[constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)

```js
class Numeric extends Number {
  constructor(value) {
    super(value);
  }
}
```

인스턴스를 생성하고 초기화하기 위한 class의 특수 메서드. **생성자 함수(constructor function)가 아니다.**


## 메서드 methods

메서드는 프로토타입에 정의되며 모든 인스턴스에서 재사용한다. 메서드는 일반 함수이거나 비동기 함수, 제네레이터, 비동기 제네레이터일 수 있다.

메서드 정의 문법은 아래와 같다:

```js
({
  property(parameters) {},
  *generator(parameters) {},
  async property(parameters) {},
  async *generator(parameters) {},

  // with computed keys
  [expression](parameters) {},
  *[expression](parameters) {},
  async [expression](parameters) {},
  async *[expression](parameters) {},
})
```

```js
class Pair {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }

  print() {
    console.log('a:', this.a);
    console.log('b:', this.b);
  }

  *gen() {
    yield this.a;
    yield this.b;
  }

  async plus() {
    return await this.a + this.b;
  }
}
```


## static

생성자 함수의 메서드 혹은 프로퍼티(필드 혹은 메서드)를 만드는 키워드. 이 키워드가 붙은 프로퍼티를 스태틱 필드 혹은 스태틱 메서드라고 한다.

```js
class Hitman {
  static yo() {
    return 'I kill you!'
  }
}
Hitman.yo();
```


## public class feature

아무 제약이 없는 단순한 프로퍼티.

```js
class Building {
  static shape = 'square';

  location = 'some-where';
  number;

  constructor(number) {
    this.number = number;
  }
}

Building.shape; // 'square' 

var building = new Building(1234);
building.location; // "some-where"
building.number; // 1234
```

그냥 특별한 표시 없이 식별자로 선언만 하면 퍼블릭 프로퍼티다. 스태틱 프로퍼티도 public class feature에 포함된다.


## private class feature

식별자 앞에 `#`를 붙이면 클래스 외부에선 참조할 수 없는 프로퍼티가 된다:

```js
class Company {
  static #ps = 'is postscript.'

  #classified = 'actually not';
  #secret;

  constructor(secret) {
    this.#secret = secret;
  }

  #privateMethod() {
    // …
  }
}

Company.#ps; // Uncaught SyntaxError: reference to undeclared private field or method #ps

var company = new Company('anyone can see this');
console.log(company); // Object { #classified: "actually not", #secret: "anyone can see this" }

company.#classified; // Uncaught SyntaxError: reference to undeclared private field or method #classified
company.#secret; // Uncaught SyntaxError: reference to undeclared private field or method #secret
company.#privateMethod(); // Uncaught SyntaxError: reference to undeclared private field or method #privateMethod;
```

프라이빗 프로퍼티는 보이긴 하지만 SyntaxError가 발생하며 접근할 수 없다. 

자바스크립트 구현체(= 브라우저 등)마다 예외가 있을 수 있다. 가령 파이어폭스 개발자도구 콘솔에선 프라이빗 프로퍼티에 접근할 수 없지만, 크롬 개발자도구 콘솔에선 잘 된다. 🤔


## extends

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends)

**TODO** 상속(확장)

### 빌트인 프로토타입의 확장

```js
Number.prototype.format = function (n, x) {
  var re = '\\d(?=(\\d{' + (x || 3) + '})+' + (n > 0 ? '\\.' : '$') + ')';
  return this.toFixed(Math.max(0, ~~n)).replace(new RegExp(re, 'g'), '$&,');
}

(123456).format(0); // "123,456"

// 소스 출처: http://stackoverflow.com/questions/149055/how-can-i-format-numbers-as-money-in-javascript
```

위는 Number 프로토타입에 `format` 함수를 추가하는 코드인데, 빌트인 프로토타입을 확장하는 것은 추천되지 않는다.

대신 아래처럼 class 문법으로 새로운 프로토타입을 정의하는 방법을 쓸 수 있다:

```js
class Numeric extends Number {
  format(n, x) {
    var re = '\\d(?=(\\d{' + (x || 3) + '})+' + (n > 0 ? '\\.' : '$') + ')';
    return this.toFixed(Math.max(0, ~~n)).replace(new RegExp(re, 'g'), '$&,');
  }
};

new Numeric(123456).format(0); // "123,456"
```


## super

**TODO**


## 클래스 vs 생성자 함수

### 생성자의 프로퍼티인 `caller`, `arguments`에 접근할 때 TypeError 발생함

```js
function Human() {}
var me = new Human();
me.constructor.caller; // null
me.constructor.arguments; // null


class Newbie {}
var noob = new Newbie();
noob.constructor.caller; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
noob.constructor.arguments; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
```
