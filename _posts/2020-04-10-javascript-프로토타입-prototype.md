---
layout: post
date: 2020-04-10 16:25:00 +0900
title: '[JavaScript] 프로토타입 Prototype'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - prototype
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [상속과 프로토타입 Inheritance and the prototype chain \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [Object \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [Object.prototypes \| MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)
- [PoiemaWeb: Prototype](https://poiemaweb.com/js-prototype)
- [JAVASCRIPT.INFO: 프로토타입과 프로토타입 상속](https://ko.javascript.info/prototypes)
- [JavaScript: 프로토타입(prototype) 이해](http://www.nextree.co.kr/p7323/)


## 개요

자바스크립트 프로토타입을 분석한 글.


## 프로토타입이란?

> 프로토타입 기반 프로그래밍은 객체지향 프로그래밍의 한 형태의 갈래로 클래스가 없고, 클래스 기반 언어에서 상속을 사용하는 것과는 다르게, 객체를 원형(프로토타입)으로 하여 복제의 과정을 통해 객체의 작동 방식을 다시 사용할 수 있다. 프로토타입기반 프로그래밍은 클래스리스(class-less), 프로토타입 지향(prototype-oriented) 혹은 인스턴스 기반(instance-based) 프로그래밍이라고도 한다.
>
> \- [위키백과 \| 프로토타입 기반 프로그래밍](https://ko.wikipedia.org/wiki/프로토타입_기반_프로그래밍)

간단히 말해 객체이면서 동시에 다른 객체의 원형이 되는 것. 클래스 기반 언어에서 클래스의 생성자를 통해 객체(혹은 인스턴스)가 생성되는 것과 다르게, 자바스크립트는 프로토타입을 복제하는 방식으로 객체를 만든다.


## 프로토타입 확인

객체(=인스턴스)의 프로토타입을 확인하는 방법은 `Object.getPrototypeOf()` 메서드를 이용하는 것과, 객체의 프로토타입을 가리키는 **비표준이지만 표준처럼 쓰이는** 프로퍼티 `Object.prototype.__proto__`를 이용하는 방법이 있다:

⚠️ `Object.prototype.__proto__`는 [지원이 중단된(deprecated)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto) 기능이다.

```js
var o = new Object();
Object.getPrototypeOf(o) === o.__proto__; // true
```

함수의 프로퍼티인 `Object.prototype`은 해당 함수로 생성된 객체의 프로토타입을 가리킨다.

가령 생성자 함수 `Mankind()`와 이 함수로 생성한 객체 `human`이 있을때:

```js
function Mankind() {}
var human = new Mankind();
```

자바스크립트는 `Mankind()` 함수에서 만들어질 객체의 부모를 내부 슬롯(internal slot)인 `human.[[Prototype]]`에 할당한다:

```js
Mankind.prototype === Object.getPrototypeOf(human); // true
Mankind.prototype === human.__proto__; // true
```


## 프로토타입 체인

![](/images/javascript-prototype-2.png)

어떤 객체의 `foo`라는 프로퍼티를 참조할 때, `foo`가 그 객체에 있을 수도 없을 수도 있다. 만약 없다면 프로토타입에서 `foo`를 찾는다. 여기에도 없다면 더 상위 프로토타입에 있는지 찾아 올라가는 것을 반복한다. 이런 과정 혹은 구조를 **프로토타입 체인**이라 하며, 자바스크립트에선 이를 이용해 상속과 확장을 구현한다.

프로토타입 체인으로 `Object` 프로토타입까지 도달했는데 여기에도 `foo`가 없다면 마참내3 `undefined`를 반환한다. `Object` 프로토타입은 최상위 프로토타입이라서 더 이상 찾아 올라갈 대상이 없기 때문.

```js
function Fruit() {}
var berry = new Fruit();
```

`berry`의 프로토타입은 `Fruit` 프로토타입이다:

```js
Object.getPrototypeOf(berry) === Fruit.prototype; // true
```

그리고 `Fruit` 프로토타입의 프로토타입은 `Object` 프로토타입이다:

```js
Object.getPrototypeOf(Object.getPrototypeOf(berry)) === Object.prototype; // true
```

마지막으로 `Object` 프로토타입의 프로토타입은 `null`이다:

```js
Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(berry))) === null; // true
```

### 프로토타입 체인의 작동 조건

프로토타입 체인은 프로퍼티의 값을 가져올 때만 작동한다. 프로퍼티에 값을 할당할 때는 객체에 프로퍼티가 있는지 확인하고, 없으면 프로토타입 체인을 탐색하는게 아니라 객체에 해당 프로퍼티를 새로 추가한다:

```js
Object.getPrototypeOf(berry).aaa = 123;
berry.aaa; // 123

berry.aaa = 456;

berry.aaa; // 456
Object.getPrototypeOf(berry).aaa; // 123
```


## .constructor

객체에는 생성자 함수를 가리키는 프로퍼티 `constructor`가 있다. 가령 다음의 경우:

```js
function Newbie() {}
var noob = new Newbie();
```

`noob.constructor`는 `Newbie()`를 가리킨다:

```js
noob.constructor === Newbie; // true
```

이것은 `noob`을 만들어낸 생성자 함수가 `Newbie()`라는 뜻이다.

### 프로토타입의 `.constructor`

`Newbie` 프로토타입의 `.constructor`도 `Newbie()`를 가리키는데:

```js
Newbie.prototype.constructor === Newbie; // true
```

이것은 **`Newbie` 프로토타입을 `Newbie()` 생성자 함수가 만들어냈다는 뜻이 아니라** 단순히 프로퍼티를 통해 연결된 것 뿐이니 오해하지 말자.

### 프로토타입 이름 알아내기

파이어폭스의 콘솔에선 프로토타입 이름이 생략되어 표시되는 인스턴스들이 있다. 이벤트 객체를 이벤트 핸들러 함수에서 출력할 때가 대표적인데, 이럴 때 `.constructor`를 활용할 수 있다:

```html
<button type="button" onclick="handleEvent(event)">보턴</button>
<script>
function handleEvent(event) {
  console.log(event.constructor.name); // MouseEvent
}
</script>
```

### 쓰잘머리 없는 정보: 생성자 함수의 생성자 함수

`Newbie()`의 생성자 함수는 `Function()`이다:

```js
Newbie.constructor === Function; // true
```

그리고 `Function()`의 생성자 함수는 `Function()`이며, `Function()`의 생성자 함수의 생성자 함수도 `Function()`이고, `Function()`의 생성자 함수의 생성자 함수의 생성자 함수도 `Function()`이고, `Function()`의 생성자의 생성자의 생성자의 생성자도 `Function()`이다. ~~고만해미친놈아~~:

```js
Function.constructor === Function; // true
Function.constructor.constructor === Function; // true
Function.constructor.constructor.constructor === Function; // true
Function.constructor.constructor.constructor.constructor === Function; // true
```

### 쓰잘머리 없는 정보: 그럼 생성자 함수의 프로토타입은?

`Function` 프로토타입은 `Function.__proto__`와 같은데, `Newbie.__proto__`와 `Newbie.prototype`이 일치하지 않는것과 비교해 차이가 있다:

⚠️ `Newbie.prototype`은 `Newbie()` 함수의 프로토타입이 아니라 `Newbie()` 함수로 만들어질 객체의 프로토타입`[[Prototype]]`임을 기억할 것. `Newbie()` 함수의 프로토타입은 `Newbie.[[Prototype]]`이라 표현한다.

```js
Function.__proto__ === Function.prototype; // true
Function.__proto__; // function ()
Function.prototype; // function ()

Newbie.__proto__ === Newbie.prototype; // false
Newbie.__proto__; // function ()
Newbie.prototype; // Object { … }
```

그리고 `Newbie()`의 프로토타입은 `Function` 프로토타입이며 `Function` 프로토타입의 프로토타입은 `Object` 프로토타입이다:

```js
Newbie.__proto__; // function ()
Newbie.__proto__ === Function.__proto__; // true
Newbie.__proto__.__proto__ === Object.prototype; // true
Function.__proto__.__proto__ === Object.prototype; // true
```

사실 몰라도 됨 😏

<!-- **TODO** 이 밑에는 왜 써놓은 걸까? -->
<!--
 ## Object.prototype과 Object()의 차이

`Object.prototype`은 `Object()` 함수로 생성된 객체의 프로토타입인 `Object` 프로토타입을 가리킨다. `Function` 생성자 함수 혹은 인스턴스를 통해 `Object.prototype`의 프로퍼티 `Object.prototype.constructor`에 접근할 수 있는데, 모든 객체의 최상위 프로토타입은 `Object` 프로토타입이기 때문이다.

```js
({}).hasOwnProperty('constructor'); // false 출력, constructor는 인스턴스의 프로퍼티가 아님
({}).constructor; // function Object() 출력, 상속 받은 프로퍼티라서 접근 가능
Function.constructor; // function Function()
```

반면 `Object()`는 생성자 함수 그 자체를 의미한다. 생성자 함수는 프로토타입이 아니므로 이 함수의 프로퍼티는 상속되지 않는다. 따라서 `Object()`의 메서드인 `Object.assign()`은 Function 객체나 인스턴스를 통해 접근할 수 없다:

```js
Function.assign; // undefined
({}).assign; // undefined
``` -->


## 프로퍼티의 가려짐 Property Shadowing

객체의 프로퍼티는 객체 자신이 소유한 프로퍼티(own properties)와 프로토타입의 프로퍼티로 나뉜다. 소유한 프로퍼티가 없고 프로토타입의 프로퍼티만 있는 객체는 해당 프로퍼티를 읽으려고 할 때 프로토타입의 것을 반환할 것이다.

만약 소유한 프로퍼티와 프로토타입의 프로퍼티가 동시에 존재한다면, 소유한 프로퍼티가 우선되며 프로토타입의 프로퍼티는 객체의 프로토타입을 통해서만 확인할 수 있는 상태가 된다. 이를 '프로퍼티의 가려짐'이라고 한다.

ℹ️ 객체가 소유한 프로퍼티인지 아닌지는 [`Object.prototype.hasOwnProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)로 확인할 수 있음.

```js
function Fn() {
  this.a = 1;
  this.b = 2;
}
var o = new Fn();

Fn.prototype.b = 3;
Fn.prototype.c = 4;

o.a; // 1
o.__proto__.a; // undefined
o.b; // 2
o.__proto__.b; // 3
o.c; // 4
o.__proto__.c; // 4
```

프로퍼티의 값으로 함수가 지정되었을 땐 메서드 오버라이딩(method overriding)이라는 용어를 사용한다. 이름만 다르고 개념은 같음.


## 객체의 프로퍼티 vs 프로토타입의 프로퍼티

프로퍼티의 소유자가 누구인지의 차이인데, 예를 들어:

```js
function HelloWorld() {
  this.sayHi = function () {
    console.log('Hi!');
  };
};
HelloWorld.prototype.sayHello = function () {
  console.log('Hello!');
};

var helloWorld = new HelloWorld();
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

프로토타입을 상속하는 방법은 여러 가지가 있다. 그 중 가장 추천하는 방법은 class인데, 실행 환경에 따라 사용 불가일 수도 있으니 적절히 다른 방법을 쓰면 된다. (IE는 클래스 문법을 지원하지 않음, `Object.create()`는 IE9부터 가능)

### [Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

웬만하면 이걸 쓰자.

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

var arr = new Arr();
arr.push('a');
arr.push('b');
arr.push('c');

arr; // Array(3) [ "a", "b", "c" ]
arr.doubleLength; // 6

arr.spitout();
arr.length; // 0
```

### [Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

`Object.create(obj[, descriptors])`는 `obj`가 프로토타입인 새로운 객체를 반환한다. 그리고 이 객체에 필요한 프로퍼티를 추가하는 방식:

```js
var grandpa = {
  age: 70
};

var daddy = Object.create(grandpa, {
  age: {
    value: 45
  }
});

var son = Object.create(daddy, {
  age: {
    value: 17
  }
});

son.__proto__.__proto__.age; // 70
son.__proto__.age; // 45
son.age; // 17
```

ℹ️ 한편 `null`을 프로토타입으로 지정하면 순수 사전식(pure dictionary) 객체를 만들 수 있다:

```js
var plainObject = Object.create(null);
plainObject.ga = '가';
plainObject.na = '나';

plainObject; // { ga: "가", na: "나" }
plainObject.__proto__; // undefined
plainObject.toString; // undefined
```

이렇게하면 `Object` 프로토타입의 프로퍼티를 하나도 상속받지 않는, 말그대로 아주 단순한 객체가 생성된다.

### [\_\_proto\_\_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#prototype_mutation)

이 방법은 상속이나 확장이 아니라 변이(mutation)라고 부른다:

```js
var obj = {};
Object.getPrototypeOf(obj) === Object.prototype; // true

var obj2 = {__proto__: null};
Object.getPrototypeOf(obj2) === null; // true
```

```js
var bomb = {
  explosion: true
};
var fatboy = {
  __proto__: bomb,
  assuredDestruction: true
};
// fatboy.__proto__ = bomb; 이렇게 하는거랑 같음

fatboy.explosion; // true
fatboy.assuredDestruction; // true
Object.getPrototypeOf(fatboy) === bomb; // true
```

앞서 말했듯이 `__proto__`는 지원이 중단된 기능인데다 속도 이슈까지 있다고 하니 사용하면 안 되지만, `Object.setPrototypeOf()`는 IE11부터 쓸 수 있어서...

### .apply + `[[Prototype]]`

모든 브라우저에서 사용 가능한 상속 방법.

```js
function Calculator(number) {
  this.number = number;
}
Calculator.prototype.getDouble = function () {
  return this.number * 2;
}
function Laptop(number) {
  Calculator.apply(this, arguments);
}
Laptop.prototype = Calculator.prototype;

var notebook = new Laptop(256);
notebook.number; // 256
notebook.getDouble() // 512
```

이 방법도 런타임 중에 프로토타입을 바꾸는거라 `__proto__`처럼 속도 이슈가 있을것 같음.


끗.
