---
layout: post
date: 2019-04-10 16:25:00 +0900
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

---

![](/images/javascript-prototype.png)

## 프로토타입이란?

> 프로토타입 기반 프로그래밍은 객체지향 프로그래밍의 한 형태의 갈래로 클래스가 없고, 클래스 기반 언어에서 상속을 사용하는 것과는 다르게, 객체를 원형(프로토타입)으로 하여 복제의 과정을 통하여 객체의 동작 방식을 다시 사용할 수 있다. 프로토타입기반 프로그래밍은 클래스리스<sup>class-less</sup>, 프로토타입 지향<sup>prototype-oriented</sup> 혹은 인스턴스 기반<sup>instance-based</sup> 프로그래밍이라고도 한다.
>
> [https://ko.wikipedia.org/wiki/프로토타입_기반_프로그래밍](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EA%B8%B0%EB%B0%98_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

간단히 말해 객체의 원형. 클래스 기반 언어에서 클래스의 생성자를 통해 객체(혹은 인스턴스)가 생성되는 것과 다르게, 자바스크립트는 프로토타입을 복제하는 방식으로 객체를 만든다.

## 프로토타입 확인

객체(=인스턴스)의 프로토타입을 확인하는 방법은 `Object.getPrototypeOf()` 메서드를 이용하는 것과, 객체의 프로토타입을 가리키는 **비표준이지만 표준처럼 쓰이는** 속성 `Object.prototype.__proto__`\*를 이용하는 방법이 있다:

\* `Object.prototype.__proto__`는 [deprecated](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)된 속성이며 사용을 권장하지 않음.

```js
let o = new Object();
Object.getPrototypeOf(o) === o.__proto__; // true
```

함수의 속성인 `Object.prototype`은 해당 함수로 생성된 객체의 프로토타입을 가리킨다. 여기서 '함수로 생성된 객체'란, **함수를 선언할 때 자동으로 생성되는 객체** 와 명시적으로 생성자 함수를 호출해 생성된 객체 모두를 말한다.

가령 생성자 함수 `Newbie`와 이 함수로 생성한 객체 `noob`이 있을때:

```js
function Newbie() {}
let noob = new Newbie();
```

자바스크립트는 `Newbie` 함수로 만든 객체를 `Newbie.prototype`에 할당한다. 이 사실은 `Newbie.prototype`의 생성자가 `Newbie`와 일치하며 `noob`의 생성자와도 일치한다는 것으로 확인할 수 있다:

```js
Newbie.prototype.constructor === Newbie; // true
Newbie.prototype.constructor === noob.constructor; // true
noob.constructor === Newbie; // true
```

그리고 함수의 `.prototype` 속성이 가리키는 프로토타입은 `noob`의 프로토타입과 일치한다:

```js
Newbie.prototype === Object.getPrototypeOf(noob); // true
Newbie.prototype === noob.__proto__; // true
```

## 프로토타입 체인

자바스크립트의 모든 객체는 자신을 만들어낸 프로토타입이 있는데, 알고 보면 그 프로토타입도 자신을 만들어낸 프로토타입이 존재한다. 이런 식의 연결은 `Object` 프로토타입까지 이어지며, 요것이 바로 _프로토타입 체인_ 이라 하는 일종의 상속과 확장 개념 되시겠다.

```js
function Animal() {}
let sheep = new Animal();
```

여기서 `Animal`로 만들어진 객체 `sheep`의 프로토타입은 `Animal.prototype`과 같다:

```js
sheep.__proto__ === Animal.prototype; // true
```

그리고 `Animal.prototype`의 프로토타입은 `Object.prototype`이다:

```js
Animal.prototype.__proto__ === Object.prototype; // true
```

마지막으로 `Object.prototype`의 프로토타입은 `null`이다. 이것은 `Object.prototype`이 최상위 프로토타입임을 의미한다:

```js
Object.prototype.__proto__ === null; // true
```

### 곁다리: 생성자 함수의 생성자

`sheep`의 생성자 함수는 `Animal`인데, `Animal`의 생성자 함수는 `Function`이다. `Function`은 `Object`의 생성자이기도 하다:

```js
sheep.constructor === Animal; // true
Animal.constructor === Function; // true
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

`Animal.__proto__`와 `Animal.prototype`이 동일하지 않는것과 비교해 차이가 있다. 그리고 `Animal` 함수 자체의 프로토타입과 `Function`의 프로토타입은 같으며, `Function` 함수의 프로토타입의 프로토타입은(?) `Object.prototype`이다:

```js
Animal.__proto__; // function ()
Animal.__proto__ === Function.__proto__; // true
Animal.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

이 정도쯤 쓰고 보니 그냥 뻘글 수준... 😏

## Object.prototype와 Object의 차이

`Object.prototype`은 `Object`로 생성된 객체의 프로토타입을 가리킨다. `Object.prototype`은 모든 객체의 원형이며 프로토타입의 속성은 상속된다. 따라서 `Object.prototype.__proto__` 속성은 객체를 통해 접근할 수 있다:

```js
Object.prototype.hasOwnProperty('__proto__'); // true
Object.prototype; // Object { … }

({}).hasOwnProperty('__proto__'); // false
({}).__proto__; // Object { … }
```

반면 `Object`는 생성자 혹은 생성자 함수 그 자체를 의미한다. 함수는 프로토타입이 아니다. 따라서 `Object.getOwnPropertyDescriptors()`는 상속되지 않으므로 생성자 함수나 인스턴스를 통해 접근할 수 없다:

```js
function Human() {}
let person = new Human();
Human.getOwnPropertyDescriptors; // undefined
person.getOwnPropertyDescriptors; // undefined
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

## 프로토타입 재정의

생성자 함수가 가리키는 프로토타입을 재정의:

```js
function Apple() {}
Apple.prototype; // {}
a = new Apple();

Apple.prototype = { isFruit: true };
Apple.prototype; // { isFruit: true }
b = new Apple();

a.isFruit; // undefined
b.isFruit; // true

a.__proto__ != b.__proto__; // true
```

객체의 프로토타입을 재정의:

```js
function Orange() {}
a = new Orange();
b = new Orange();
a.__proto__ === b.__proto__; // true

b.__proto__ = { isFruit: true }
a.isFruit; // undefined
b.isFruit; // true
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

### 꼐속...

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

첫 번째 코드는 인스턴스마다 함수가 생성되고, 두 번째 코드는 인스턴스에 관계없이 프로토타입의 메서드로 딱 하나만 생성된다.
