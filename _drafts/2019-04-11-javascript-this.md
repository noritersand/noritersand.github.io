---
layout: post
date: 2019-04-11 11:15:00 +0900
title: '[JavaScript] this'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - this
---

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)


## this란 무엇인가

블라블라


## 자바스크립트 객체 메서드 내에서 this

```js
var owner = {
  fn: function() {
    return this;
  }
};

owner === owner.fn();
```

여기서 `this`는 함수를 소유한 객체다.


## 화살표 함수의 this

화살표 함수의 this는 함수의 소유자가 아니라 전역 객체다:

```js
var obj = {
  do: () => {
    console.log('do this:', this); // window
  },
  do2() {
    console.log('do2 this:', this); // obj
  }
};

var obj2 = {
  fn() {
    console.log(this); // 이 this는 obj2
    setTimeout(function() {
      console.log(this); // 이 this는 window
    }, 500);
  },
  fn2() {
    console.log(this); // 이 this는 obj2
    setTimeout(() => {
      console.log(this); // 이 this도 obj2
    }, 500);
  }
}
```


## 객체와 프로토타입의 this

**TODO: 아래 코드는 퍼왔으니 수정해**

```js
// animal엔 다양한 메서드가 있습니다.
var animal = {
  walk() {
    if (!this.isSleeping) {
      alert(`동물이 걸어갑니다.`);
    }
  },
  sleep() {
    console.log('Zzz..');
    this.isSleeping = true;
  }
};

var rabbit = {
  name: "하얀 토끼",
  __proto__: animal
};

// rabbit의 프로퍼티 isSleeping을 true로 변경합니다.
rabbit.sleep();

console.log(rabbit.isSleeping); // true
console.log(animal.isSleeping); // undefined (프로토타입에는 isSleeping이라는 프로퍼티가 없다)

animal.sleep();
console.log(animal.isSleeping); // true (이제 있음)
```

위 예시에서 `this`는 상속된 메서드인지 아닌지가 중요한게 아니라 `.` 앞의 객체가 누구인지에 따라 달라짐.
