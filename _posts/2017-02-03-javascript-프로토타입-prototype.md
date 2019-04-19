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

#### 관련 문서

- [MDN: 상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [MDN: Object.prototype](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)
- [MDN: Object​.prototype​.constructor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)

![](/images/javascript-prototype.png)

## 프로토타입이란?

간단히 말해 객체의 원형. 자바에서 클래스 기반으로 객체(혹은 인스턴스)가 만들어지듯, 자바스크립트에선 프로토타입을 기반으로 객체가 만들어진다.

## 프로토타입 체인

자바스크립트의 모든 객체는 프로토타입이 있고, 이 프로토타입이 의미하는 객체에도 프로토타입이 있다. 최초의 프로토타입으로부터 이어지는 일종의 상속 개념인데, 이것을 프로토타입 체인이라고 한다.

아래 같은 **생성자 함수** `Newbie`가 있을 때:

```js
function Newbie() {
  if (!(this instanceof Newbie)) {
    return new Newbie(name);
  }
  this.trait = 'Know nothing';
}

let noob = new Newbie();
```

`Newbie`로 만들어진 객체 `noob`의 프로토타입은 `Newbie.prototype`이다. (다시 언급하지만 `Newbie`는 생성자 함수다.)

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

방향을 살짝 틀어서 `Newbie`의 프로토타입과 `Function`의 프로토타입은 `function ()`를 가리킨다. (이게 정확히 뭔지는 몲) `function ()`의 프로토타입은 `Object.prototype`이다:

```js
Newbie.__proto__; // function ()
Newbie.__proto__ === Function.__proto__; // true
Newbie.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

복잡하면 그냥 이 글 맨 위의 (그리는데 20분 걸린) 그림을 보자.

## 속성의 가려짐 property shadowing

TODO

## 메서드 오버라이딩 method overriding

TODO

## ETC

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

위 둘의 결과는 같으나 미묘하게 다르다. ~~그게뭔소리여시방~~ 첫 번째 코드는 인스턴스마다 메서드가 생성되고, 두 번째 코드는 인스턴스에 관계없이 프로토타입의 메서드 딱 하나만 생성된다.
