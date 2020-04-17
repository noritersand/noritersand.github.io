---
layout: post
date: 2019-04-21 22:29:00 +0900
title: '[JavaScript] 클래스 class'
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

- [MDN: Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
- [PoiemaWeb: 클래스](https://poiemaweb.com/es6-class)
- [JAVASCRIPT.INFO: 클래스와 기본 문법](https://ko.javascript.info/class)

#### 브라우저 호환

- IE에서 사용 불가

[ES2015(ES6)](https://www.ecma-international.org/ecma-262/6.0/#sec-class-definitions)에서 소개된 생성자 함수를 대체하는 새로운 문법이다. 구문들의 의미가 분명해서 사용하긴 편하지만 IE 때문에 웹에서 쓸 수 있는 날은 멀었다.

MDN의 설명에 따르면 class도 사실은 함수라서 사용 방법이 함수처럼 선언문과 표현식으로 나뉜다.

## class 선언

```js
class Newbie {
  constructor(trait) {
    this._trait = trait;
  }
  get trait() {
    return this._trait || 'know nothing';
  }
  set trait(arg) {
    this._trait = arg;
  }
  levelUp() {
    console.log('I feel stronger.');
    this._trait = 'barely shooting an arrow';
  }
}

let noob = new Newbie();

noob.trait; // "know nothing"
noob.levelUp(); // I feel stronger.
noob.trait; // "barely shooting an arrow"
noob.trait = '12 hours continuous sleep';
noob.trait; // "12 hours continuous sleep"
```

함수 선언과 다르게 끌어올림은 발생하지 않는다:

```js
let magicUser = new Sorcerer(); // ReferenceError: can't access lexical declaration `Sorcerer' before initialization

class Sorcerer {}
```

## class 표현식

TODO

## static

TODO

## super

TODO

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

### 생성자의 프로퍼티인 `caller`, `arguments`에 접근할 수 없다.

```js
class Test {}
let t = new Test();
t.constructor.caller; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
t.constructor.arguments; // TypeError: "'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them"
```
