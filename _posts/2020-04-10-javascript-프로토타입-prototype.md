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

## 프로토타입이란?

> 프로토타입 기반 프로그래밍은 객체지향 프로그래밍의 한 형태의 갈래로 클래스가 없고, 클래스 기반 언어에서 상속을 사용하는 것과는 다르게, 객체를 원형(프로토타입)으로 하여 복제의 과정을 통하여 객체의 동작 방식을 다시 사용할 수 있다. 프로토타입기반 프로그래밍은 클래스리스<sup>class-less</sup>, 프로토타입 지향<sup>prototype-oriented</sup> 혹은 인스턴스 기반<sup>instance-based</sup> 프로그래밍이라고도 한다.
>
> [위키백과: 프로토타입 기반 프로그래밍](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EA%B8%B0%EB%B0%98_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

간단히 말해 객체이면서 동시에 다른 객체의 원형이 되는 것. 클래스 기반 언어에서 클래스의 생성자를 통해 객체(혹은 인스턴스)가 생성되는 것과 다르게, 자바스크립트는 프로토타입을 복제하는 방식으로 객체를 만든다.

## 프로토타입 확인

객체(=인스턴스)의 프로토타입을 확인하는 방법은 `Object.getPrototypeOf()` 메서드를 이용하는 것과, 객체의 프로토타입을 가리키는 **비표준이지만 표준처럼 쓰이는** 프로퍼티 `Object.prototype.__proto__`\*를 이용하는 방법이 있다:

\* `Object.prototype.__proto__`는 지원이 중단된([deprecated](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)) 기능이다.

```js
let o = new Object();
Object.getPrototypeOf(o) === o.__proto__; // true
```

함수의 프로퍼티인 `Object.prototype`은 해당 함수로 생성된 객체의 프로토타입을 가리킨다. 여기서 '함수로 생성된 객체'란, **함수를 선언할 때 자동으로 생성되는 객체** 와 명시적으로 생성자 함수를 호출해 생성된 객체 모두를 말한다.

가령 생성자 함수 `Mankind()`와 이 함수로 생성한 객체 `human`이 있을때:

```js
function Mankind() {}
let human = new Mankind();
```

자바스크립트는 `Mankind()` 함수에서 자동으로 만들어진 객체를 `Mankind.prototype`에 할당한다. 이 사실은 `Mankind.prototype`의 생성자가 `Mankind()`와 일치하며 `human`의 생성자와도 일치한다는 것으로 확인할 수 있다:

```js
Mankind.prototype.constructor === Mankind; // true
Mankind.prototype.constructor === human.constructor; // true
human.constructor === Mankind; // true
```

그리고 `ManKind.prototype`은 `human`의 프로토타입과 일치한다:

```js
Mankind.prototype === Object.getPrototypeOf(human); // true
Mankind.prototype === human.__proto__; // true
```

## 프로토타입 체인

![](/images/javascript-prototype-2.png)

자바스크립트의 모든 객체는 자신의 원형인 부모 객체가 있다. 부모도 객체이므로 당연히 부모가 있고, 이론상 부모의 부모의 부모의 부모의 부모의 ~~고만해~~ 부모도 있을 수 있지만, 사실 이렇게까지 조상이 많은 경우는 드물고 조부모나 증조부 쯤에서 최상위 객체인 `Object` 프로토타입을 만나 연결이 끝난다.

객체의 프로퍼티를 참조할 때 해당 프로퍼티가 객체에 없으면 객체의 프로토타입에서 찾는다. 프로토타입에도 없으면 더 위에 있는 프로토타입에서 찾는다. 이것을 **프로토타입 체인** 이라 하며, 자바스크립트에선 이를 이용해 상속과 확장을 구현한다. 덧붙여서, 만약 `Object` 프로토타입에도 찾는 프로퍼티가 없으면 `undefined`를 반환한다. 왜냐면 `Object` 프로토타입의 프로토타입은 `null`이라서 더 이상 찾을 대상이 없기 때문.

```js
function Newbie() {}
let noob = new Newbie();
```

여기서 생성자 함수 `Newbie()`로 만들어진 객체 `noob`의 프로토타입은 `Newbie.prototype`과 같다:

```js
noob.__proto__ === Newbie.prototype; // true
```

그리고 `noob.__proto__`의 프로토타입은 `Object.prototype`이다:

```js
noob.__proto__.__proto__ === Object.prototype; // true
```

마지막으로 `Object.prototype`의 프로토타입은 `null`이다:

```js
Object.prototype.__proto__ === null; // true
```

### 프로토타입 체인의 작동 조건

프로토타입 체인은 프로퍼티의 값을 가져올 때만 작동한다. 프로퍼티에 값을 할당할 때는 객체에 프로퍼티가 있는지 확인하고, 없으면 프로토타입 체인을 탐색하는게 아니라 객체에 해당 프로퍼티를 새로 추가한다:

```js
Object.prototype.aaa = 123;
noob.aaa; // 123

noob.aaa = 456;

noob.aaa; // 456
noob.__proto__.aaa; // 123
```

### 곁다리: 생성자 함수의 생성자

`Newbie()`의 생성자는 `Function()`이다:

```js
Newbie.constructor === Function; // true
```

그리고 `Function()`의 생성자는 `Function()`이며, `Function()`의 생성자의 생성자도 `Function()`이고, `Function()`의 생성자의 생성자의 생성자도 `Function()`이고, `Function()`의 생성자의 생성자의 생성자의 생성자도 `Function()`이다. ~~고만해미친놈아~~:

```js
Function.constructor === Function; // true
Function.constructor.constructor === Function; // true
Function.constructor.constructor.constructor === Function; // true
Function.constructor.constructor.constructor.constructor === Function; // true
```

### 곁다리: 그럼 생성자 함수의 프로토타입은?

`Function` 프로토타입은 `Function.__proto__`와 같은데, `Newbie.__proto__`와 `Newbie.prototype`이 일치하지 않는것\*과 비교해 차이가 있다:

\* **주의**: `Newbie.prototype`은 `Newbie()` 함수의 프로토타입이 아니라 `Newbie()` 함수로 만들어질 객체의 프로토타입임을 기억할 것. `Newbie()` 함수의 프로토타입은 `Newbie.__proto__`가 가리킨다.

```js
Function.__proto__ === Function.prototype; // true
Function.__proto__; // function ()
Function.prototype; // function ()

Newbie.__proto__ === Newbie.prototype; // false
Newbie.__proto__; // function ()
Newbie.prototype; // Object { … }
```

그리고 `Newbie()`의 프로토타입은 `Function` 프로토타입이며 `Function` 프로토타입의 프로토타입은 `Object.prototype`이다:

```js
Newbie.__proto__; // function ()
Newbie.__proto__ === Function.__proto__; // true
Newbie.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

이 정도쯤 쓰고 보니 그냥 뻘글 수준... 😏

## Object.prototype과 Object()의 차이

`Object.prototype`은 `Object()` 함수로 생성된 객체의 프로토타입을 가리킨다. `Object.prototype`은 모든 객체의 원형이며 프로토타입의 프로퍼티는 상속된다. 따라서 `Object.prototype.__proto__`는 함수\*나 객체를 통해 접근할 수 있다:

\* 함수도 객체임

```js
({}).hasOwnProperty('__proto__'); // false
({}).__proto__; // Object { … }
Function.__proto__; // function ()
```

반면 `Object()`는 생성자 혹은 생성자 함수 그 자체를 의미한다. 함수는 프로토타입이 아니므로 함수의 프로퍼티는 상속되지 않는다. 따라서 `Object.getOwnPropertyDescriptors()`는 함수나 객체를 통해 접근할 수 없다:

```js
Function.hasOwnProperty('getOwnPropertyDescriptors'); // false
Function.getOwnPropertyDescriptors; // undefined

({}).hasOwnProperty('getOwnPropertyDescriptors'); // false
({}).getOwnPropertyDescriptors; // undefined
```

## 프로퍼티의 가려짐 property shadowing

객체의 프로퍼티는 자기만의 프로퍼티<sup>own properties</sup>\*와 프로토타입의 프로퍼티로 나뉜다. 자기만의 프로퍼티가 없고 프로토타입의 프로퍼티만 있는 객체는 해당 프로퍼티를 읽으려고 할 때 프로토타입의 것을 반환할 것이다.

만약 자기만의 프로퍼티와 프로토타입의 프로퍼티가 동시에 존재한다면, 자기만의 프로퍼티가 우선되며 프로토타입의 프로퍼티는 객체의 프로토타입을 통해서만 확인할 수 있는 상태가 된다. 이를 '프로퍼티의 가려짐'이라고 한다.

\* 자기만의 프로퍼티는 `Object.getOwnPropertyDescriptors(object)`로 확인할 수 있음.

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

프로퍼티의 값으로 함수가 지정되었을 땐 메서드 오버라이딩<sup>method overriding</sup>이라는 용어를 사용한다. 이름만 다르고 개념은 같음.

## 객체의 프로퍼티 vs 프로토타입의 프로퍼티

프로퍼티의 소유자가 누구인지의 차이인데, 예를 들어:

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

`sayHi()`는 객체의 프로퍼티, `sayHello()`는 프로토타입의 프로퍼티다.

성능 면에서 보면, 객체의 프로퍼티는 인스턴스마다 각각 만들어지고 프로토타입의 프로퍼티는 인스턴스의 수와 상관없이 딱 한 번만 만들어지므로(프로토타입이란 놈이 원래 하나만 존재함) 프로토타입 쪽이 유리하다고 할 수 있다.

## 프로토타입 재정의

객체의 프로토타입을 변경하는 행위.  
생성자 함수 `Orange()`와 `Tomato()`가 있을 때:

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

추천#1

```js
class Arr extends Array {
  get doubleLength() {
    return this.length * 2
  }
  spitout() {
    while (this.length) {
      this.pop();
    }
  }
}

let arr = new Arr();
arr.push('a');
arr.push('b');
arr.push('c');

arr; // Array(3) [ "a", "b", "c" ]
arr.doubleLength; // 6

arr.spitout();
arr.length; // 0
```

### [Object.create()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

`Object.create(obj[, descriptors])`는 `obj`가 프로토타입인 새로운 객체를 반환한다. 그리고 이 객체에 필요한 프로퍼티를 추가하는 방식:

```js
let grandpa = {
  age: 70
};

let daddy = Object.create(grandpa, {
  age: {
    value: 45
  }
});

let son = Object.create(daddy, {
  age: {
    value: 17
  }
});

son.__proto__.__proto__.age; // 70
son.__proto__.age; // 45
son.age; // 17
```

역으로 생각해서 `null`을 프로토타입으로 지정하면 순수 사전식<sup>pure dictionary</sup> 객체를 만들 수 있다:

```js
let plainObject = Object.create(null);
plainObject.ga = '가';
plainObject.na = '나';

plainObject; // { ga: "가", na: "나" }
plainObject.__proto__; // undefined
plainObject.toString; // undefined
```

이렇게하면 `Object` 프로토타입의 프로퍼티를 하나도 상속받지 않는, 말그대로 아주 단순한 객체가 생성된다.

### \_\_proto\_\_

```js
let bomb = {
  explosion: true
};
let fatboy = {
  __proto__: bomb,
  assuredDestruction: true
};
// fatboy.__proto__ = bomb; 이렇게 하는거랑 같음

fatboy.explosion; // true
fatboy.assuredDestruction; // true
```

앞서 말했듯이 `__proto__`는 지원이 중단된 기능인데다 속도 이슈까지 있다고 하니 사용하면 안되지만, `Object.setPrototypeOf()`는 IE11부터 쓸 수 있어서...

### .apply + .prototype

추천#2 모든 브라우저에서 사용 가능한 상속 방법.

```js
function Calculator(number) {
  this.number = number;
}
Calculator.prototype.getDouble = function() {
  return this.number * 2;
}
function Laptop(number) {
  Calculator.apply(this, arguments);
}
Laptop.prototype = Calculator.prototype;

let notebook = new Laptop(256);
notebook.number; // 256
notebook.getDouble() // 512
```

이 방법도 런타임 중에 프로토타입을 바꾸는거라 `__proto__`처럼 속도 이슈가 있을것 같음.
