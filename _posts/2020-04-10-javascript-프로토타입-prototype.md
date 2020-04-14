---
layout: post
date: 2020-04-10 16:25:00 +0900
title: '[JavaScript] 프로토타입 Prototype'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - prototype
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: 상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [MDN: Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [MDN: Object.prototype](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)
- [PoiemaWeb: Prototype](https://poiemaweb.com/js-prototype)
- [JAVASCRIPT.INFO: 프로토타입과 프로토타입 상속](https://ko.javascript.info/prototypes)
- [JavaScript: 프로토타입(prototype) 이해](http://www.nextree.co.kr/p7323/)

위에 링크한 한국어 문서들에서 property를 속성이라고 표현하는데, 왜 이렇게 번역했는지는 잘 몲겠음. 확실한건 자바스크립트에 attribute란 개념은 없다는 것이다. 생각해보면 property를 자산이나 특성이라 번역하면 어감이 이상하긴 함. 그래서 이 글에서도 그냥 속성이라 적는다.

## 프로토타입이란?

> 프로토타입 기반 프로그래밍은 객체지향 프로그래밍의 한 형태의 갈래로 클래스가 없고, 클래스 기반 언어에서 상속을 사용하는 것과는 다르게, 객체를 원형(프로토타입)으로 하여 복제의 과정을 통하여 객체의 동작 방식을 다시 사용할 수 있다. 프로토타입기반 프로그래밍은 클래스리스<sup>class-less</sup>, 프로토타입 지향<sup>prototype-oriented</sup> 혹은 인스턴스 기반<sup>instance-based</sup> 프로그래밍이라고도 한다.
>
> [위키백과: 프로토타입 기반 프로그래밍](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EA%B8%B0%EB%B0%98_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

간단히 말해 객체의 원형. 클래스 기반 언어에서 클래스의 생성자를 통해 객체(혹은 인스턴스)가 생성되는 것과 다르게, 자바스크립트는 프로토타입을 복제하는 방식으로 객체를 만든다.

## 프로토타입 확인

객체(=인스턴스)의 프로토타입을 확인하는 방법은 `Object.getPrototypeOf()` 메서드를 이용하는 것과, 객체의 프로토타입을 가리키는 **비표준이지만 표준처럼 쓰이는** 속성 `Object.prototype.__proto__`\*를 이용하는 방법이 있다:

\* `Object.prototype.__proto__`는 [deprecated](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) 상태이며 사용을 권장하지 않음.

```js
let o = new Object();
Object.getPrototypeOf(o) === o.__proto__; // true
```

함수의 속성인 `Object.prototype`은 해당 함수로 생성된 객체의 프로토타입을 가리킨다. 여기서 '함수로 생성된 객체'란, **함수를 선언할 때 자동으로 생성되는 객체** 와 명시적으로 생성자 함수를 호출해 생성된 객체 모두를 말한다.

가령 생성자 함수 `Human`와 이 함수로 생성한 객체 `person`이 있을때:

```js
function Human() {}
let person = new Human();
```

자바스크립트는 `Human` 함수로 만든 객체를 `Human.prototype`에 할당한다. 이 사실은 `Human.prototype`의 생성자가 `Human`와 일치하며 `person`의 생성자와도 일치한다는 것으로 확인할 수 있다:

```js
Human.prototype.constructor === Human; // true
Human.prototype.constructor === person.constructor; // true
person.constructor === Human; // true
```

그리고 함수의 `.prototype` 속성이 가리키는 프로토타입은 `person`의 프로토타입과 일치한다:

```js
Human.prototype === Object.getPrototypeOf(person); // true
Human.prototype === person.__proto__; // true
```

## 프로토타입 체인

![](/images/javascript-prototype.png)

자바스크립트의 모든 객체는 자신을 만들어낸 프로토타입이 있는데, 알고 보면 그 프로토타입도 자신을 만들어낸 프로토타입이 존재한다. 이런 식의 연결은 `Object` 프로토타입까지 이어지며, 요것이 바로 _프로토타입 체인_ 이라 하는 일종의 상속과 확장 개념 되시겠다.

```js
function Newbie() {}
let noob = new Newbie();
```

여기서 `Newbie`로 만들어진 객체 `noob`의 프로토타입은 `Newbie.prototype`과 같다:

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

### 곁다리: 생성자 함수의 생성자

`noob`의 생성자 함수는 `Newbie`인데, `Newbie`의 생성자 함수는 `Function`이다. `Function`은 `Object`의 생성자이기도 하다:

```js
noob.constructor === Newbie; // true
Newbie.constructor === Function; // true
Function === Object.constructor; // true
```

그리고 `Function`의 생성자는 `Function`이며, `Function`의 생성자의 생성자도 `Function`이고, `Function`의 생성자의 생성자의 생성자도 `Function`이고, `Function`의 생성자의 생성자의 생성자의 생성자도 `Function`이다. ~~고만해미친놈아~~:

```js
Function.constructor === Function; // true
Function.constructor.constructor === Function; // true
Function.constructor.constructor.constructor === Function; // true
Function.constructor.constructor.constructor.constructor === Function; // true
```

### 곁다리: 그럼 생성자 함수의 프로토타입은?

`Function` 함수의 프로토타입은 `Function.__proto__`와 같은데:

```js
Function.__proto__ == Function.prototype; // true
```

`Newbie.__proto__`와 `Newbie.prototype`이 동일하지 않는것과 비교해 차이가 있다. 그리고 `Newbie` 함수 자체의 프로토타입과 `Function`의 프로토타입은 같으며, `Function` 함수의 프로토타입의 프로토타입은(?) `Object.prototype`이다:

```js
Newbie.__proto__; // function ()
Newbie.__proto__ === Function.__proto__; // true
Newbie.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

이 정도쯤 쓰고 보니 그냥 뻘글 수준... 😏

## Object.prototype와 Object의 차이

`Object.prototype`은 `Object`로 생성된 객체의 프로토타입을 가리킨다. `Object.prototype`은 모든 객체의 원형이며 프로토타입의 속성과 메서드는 상속된다. 따라서 `Object.prototype.__proto__`는 객체를 통해 접근할 수 있다:

```js
({}).hasOwnProperty('__proto__'); // false
({}).__proto__; // Object { … }
```

반면 `Object`는 생성자 혹은 생성자 함수 그 자체를 의미한다. 함수는 프로토타입이 아니므로 함수의 속성과 메서드는 상속되지 않는다. 따라서 `Object.getOwnPropertyDescriptors()`는 생성자 함수나 인스턴스를 통해 접근할 수 없다:

```js
Function.hasOwnProperty('getOwnPropertyDescriptors'); // false
Function.getOwnPropertyDescriptors; // undefined

({}).hasOwnProperty('getOwnPropertyDescriptors'); // false
({}).getOwnPropertyDescriptors; // undefined
```

## 속성의 가려짐 property shadowing

객체의 속성은 자기만의 속성<sup>own properties</sup>\*과 프로토타입의 속성으로 나뉜다. 자기만의 속성이 없고 프로토타입의 속성만 있는 객체는 해당 속성을 읽으려고 할 때 프로토타입의 것을 반환할 것이다.

만약 자기만의 속성과 프로토타입의 속성이 동시에 존재한다면, 자기만의 속성이 우선되며 프로토타입의 속성은 객체의 프로토타입을 통해서만 확인할 수 있는 상태가 된다. 이를 '속성의 가려짐'이라고 한다.

\* 자기만의 속성은 `Object.getOwnPropertyDescriptors(object)`로 확인할 수 있음.

```js
function Fn() {
  this.a = 1;
  this.b = 2;
}
let o = new Fn();

Fn.prototype.b = 3;
Fn.prototype.c = 4;

o.a; // 1
o.__proto__.a; // undefined
o.b; // 2
o.__proto__.b; // 3
o.c; // 4
o.__proto__.c; // 4
```

속성의 값으로 함수로 지정되었을 땐 메서드 오버라이딩<sup>method overriding</sup>이라는 용어를 사용한다.

## 프로토타입 재정의

객체의 프로토타입을 변경하는 행위.

생성자 함수 `Orange`와 `Tomato`가 있을 때:

```js
function Orange() {}
Orange.prototype.fruit = true;

function Tomato() {}
Tomato.prototype.fruit = false;

a = new Orange(), b = new Orange();
a.fruit; // true
b.fruit; // true
```

인스턴스가 참조하는 프로토타입을 바꾸거나:

```js
b.__proto__ = Tomato.prototype;
b.fruit; // false
```

생성자 함수가 참조하는 프로토타입을 바꾸는 방식이 있다:

```js
Tomato.prototype = Orange.prototype;
c = new Tomato();
c.fruit; // true
```

## 프로토타입 상속(확장)

프로토타입을 상속하는 방법은 여러가지가 있다. 그 중 가장 추천하는 방법은 class인데, 실행 환경에 따라 사용 불가일 수도 있으니 적절히 다른 방법을 쓰면 된다. (IE는 클래스 문법을 지원하지 않음, `Object.create()`는 IE9부터 가능)

### [class](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)

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

### [Object.create()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

`Object.create(obj)`는 `obj`가 프로토타입인 새로운 객체를 반환한다. 그리고 이 객체에 필요한 속성을 추가하는 방식:

```js
let grandpapa = {
  age: 70
};

let daddy = Object.create(grandpapa);
daddy.age = 45;

let son = Object.create(daddy);
son.age = 17;

son.__proto__.__proto__.age; // 70
son.__proto__.age; // 45
son.age; // 17
```

### \_\_proto\_\_

`class`와 `Object.create()`를 사용할 수 없는 환경에선 프로토타입 재정의 항목에서 언급한 방법을 활용한다:

```js
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};

rabbit.__proto__ = animal;

rabbit.eats; // true
rabbit.jumps; // true
```

앞서 말했듯이 `__proto__`는 deprecated 속성이라서 쓰면 안되지만 `Object.setPrototypeOf()`는 IE11부터 가능한지라... 그리고 이런식의 상속은 성능 문제가 있다고 한다.

## 객체의 속성 vs 프로토타입의 속성

속성의 소유자가 누구인지의 차이인데, 예를 들어:

```js
function HelloWorld() {
  this.sayHi = function() {
    console.log('Hi!');
  };
};
HelloWorld.prototype.sayHello = function() {
  console.log('Hello!');
};

let helloWorld = new HelloWorld();
helloWorld.sayHi(); // "Hi!"
helloWorld.sayHello(); // "Hello!"
```

여기서 `sayHi()`는 객체의 속성, `sayHello()`는 프로토타입의 속성이다.

성능 면에서 보면, 객체의 속성은 인스턴스마다 각각 만들어지고 프로토타입의 속성은 인스턴스의 수와 상관없이 딱 한 번만 만들어지므로(프로토타입이란 놈이 원래 하나만 존재함) 프로토타입 쪽이 유리하다고 할 수 있다.
