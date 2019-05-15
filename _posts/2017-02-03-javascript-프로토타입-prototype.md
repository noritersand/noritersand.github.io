---
layout: post
date: 2017-02-03 13:44:00 +0900
title: 'JavaScript: 프로토타입 Prototype'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - prototype
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN: 상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [MDN: Object.prototype](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)
- [MDN: Object​.prototype​.constructor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)
- [JavaScript: 프로토타입(prototype) 이해](http://www.nextree.co.kr/p7323/)

![](/images/javascript-prototype.png)

## 프로토타입이란?

간단히 말해 객체의 원형. 자바에서 클래스 기반으로 객체(혹은 인스턴스)가 만들어지듯, 자바스크립트에선 프로토타입을 기반으로 객체가 만들어진다.

## 프로토타입 체인

자바스크립트의 모든 객체는 자신을 만들어낸 프로토타입이 있는데, 해당 프로토타입 또한 원형이 되는 프로토타입에서 파생된 결과물이다. 최초의 프로토타입으로부터 이어지는 일종의 상속과 확장의 개념이며, 이것을 프로토타입 체인이라고 한다.

아래 같은 **생성자 함수** `Newbie`가 있을 때:

```js
function Newbie() {
  if (!(this instanceof Newbie)) {
    return new Newbie(name);
  }
  this.trait = 'know nothing';
}

let noob = new Newbie();
```

`Newbie`로 만들어진 객체 `noob`의 프로토타입은 `Newbie.prototype`이다.

```js
noob.__proto__ === Newbie.prototype; // true
```

그리고 `Newbie.prototype`의 프로토타입은 `Object.prototype`이다:

```js
Newbie.prototype.__proto__ === Object.prototype; // true
```

마지막으로 `Object.prototype`의 프로토타입은 `null`이다. 이것은 `Object.prototype`이 최상위 프로토타입임을 의미한다:

```js
Object.prototype.__proto__ === null; // true
```

### 곁다리: 그럼 생성자 함수의 프로토타입은?

`noob`의 생성자 함수는 `Newbie`인데, `Newbie`의 생성자 함수는 `Function`이다:

```js
noob.constructor === Newbie; // true
Newbie.constructor === Function; // true
```

그리고 `Function`의 생성자 함수는 `Function`이며, `Function`의 생성자 함수의 생성자 함수도 `Function`이고, `Fuction`의 생성자 함수의 생성자 함수의 생성자 함수도 `Function`이고, `Fuction`의 생성자 함수의 생성자 함수의 생성자 함수의 생성자 함수도 `Function`이다~~고만해미친놈아~~:

```js
Function.constructor === Function; // true
Function.constructor.constructor === Function; // true
Function.constructor.constructor.constructor === Function; // true
Function.constructor.constructor.constructor.constructor === Function; // true
```

방향을 살짝 틀어서 `Newbie`의 프로토타입과 `Function`의 프로토타입은 `function ()`(이게 정확히 뭔지는 몲. 아마 익명 함수?)를 가리킨다. `function ()`의 프로토타입은 `Object.prototype`이다:

```js
Newbie.__proto__; // function ()
Newbie.__proto__ === Function.__proto__; // true
Newbie.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

복잡하면 그냥 이 글 맨 위의 그림을 보자.

## 속성의 가려짐 property shadowing

객체는 '자기만의 속성(own properties)'과 프로토타입의 속성이 동시에 존재할 수 있다. 이 경우 '자기만의 속성'이 우선되며, 프로토타입의 속성은 객체의 프로토타입을 통해서만 확인할 수 있는 상태가 된다. 이를 '속성의 가려짐'이라고 한다.

```js
let f = function() {
    this.a = 1;
    this.b = 2;
}
let o = new f();

f.prototype.b = 3;
f.prototype.c = 4;

o.a; // 1
o.__proto__.a; // undefined
o.b; // 2
o.__proto__.b; // 3
o.c; // 4
o.__proto__.c; // 4
```

## 메서드 오버라이딩 method overriding

TODO

## 프로토타입 확장

### class를 사용한 확장

```js
class Arr extends Array {
  get doubleLength() {
    return this.length * 2
  }
}
Arr.prototype.spitout = function() {
  while (this.length) {
    this.pop();
  }
}

let arr = new Arr();
arr.push('a');
arr.push('b');
arr.push('c');

console.log(arr); // Array(3) [ "a", "b", "c" ]
console.log(arr.doubleLength); // 6

arr.spitout();
console.log(arr.length); // 0
```

## 프로토타입의 함수

```js
let HelloWorld = function(word) {
  this.word = word;
  this.say = function() {
    console.log(word);
  };
};

let helloWorld = new HelloWorld('hi');
helloWorld.say();
```

```js
function HelloWorld(word) {
  this.word = word;
};

HelloWorld.prototype.say = function() {
  console.log(this.word);
};

let helloWorld = new HelloWorld('hi');
helloWorld.say();
```

첫 번째 코드는 인스턴스마다 함수가 생성되고, 두 번째 코드는 인스턴스에 관계없이 프로토타입의 멤버로 딱 하나만 생성된다.
