---
layout: post
date: 2020-04-08 12:28:00 +0900
title: '[JavaScript] 제네레이터 function*, generator function'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - function*
  - generator
  - generator-function
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Generator \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [function\* \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
- [https://wonism.github.io/javascript-generator/](https://wonism.github.io/javascript-generator/)
- [https://meetup.nhncloud.com/posts/73](https://meetup.nhncloud.com/posts/73)

#### 브라우저 호환

- IE 제외 모두 지원함


## 개요

ES2015부터 사용 가능한 제네레이터에 대한 간단 정리 글.


## generator function

```
function* name([param[, param[, ... param]]]) {
  statements
}
```

generator function은 호출 시 코드를 실행하는 대신 `Generator` 객체를 반환한다. 이후 `Generator.prototype.next()`가 호출되면 그때서야 `yield`를 만날 때까지 실행하며 `yield` 우측의 표현식을 평가해 반환한다.

```js
function* generate(n) {
  console.log('Hello');
  yield n; // 첫 next()에 여기까지 실행
  yield n * 2; // 두 번째 next()에 여기까지
  return n * 3; // 세 번째 next()에 여기까지 실행함.
}

var gen = generate(10); // console.log('Hello')는 아직 실행되지 않는다.

console.log(gen.next()); // { value: 10, done: false }
console.log(gen.next()); // { value: 20, done: false }
console.log(gen.next()); // { value: 30, done: true }
```


## 메서드 리터럴

```js
var obj = {
  generate: function* () {}
};
```

ES2015의 [메서드 단축 표기법](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#method_definitions)으로는 이렇게 쓴다:

```js
var obj = {
  *generate() {}
};
```


## 생성자로 사용 불가

generator function은 (엄격 모드가 아니어도) 생성자로 사용할 수 없다.

```js
function* fn() {}
var gen = new fn; // TypeError: f is not a constructor
gen = new fn(); // TypeError: f is not a constructor
```


## 이터레이터

제네레이터 함수는 항상 이터레이터 객체를 반환한다. 이 이터레이터 객체에는 인스턴스 메서드 `next()`, `return()`, `throw()`가 있는데, 이 메서드들로 제네레이터 함수의 실행을 제어하는 식이다.

**TODO** 추가 설명

### for-of 에서의 활용

제네레이터 함수가 반환하는 Generator 객체는 Iterator 프로토콜을 따르기 때문에 for-of 루프에서 반복할 수 있다:

```js
function* myGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = myGenerator(); // Generator 객체 반환
for (const value of gen) {
  console.log(value); // 1, 2, 3 출력
}
```


## 비동기 작업의 관리

제네레이터 함수에는 비동기 기능이 없는데 제네레이터 함수의 장점이나 기능을 설명할 때 '비동기'라는 말이 언급된다. 이것은 주로 제네레이터가 비동기 작업을 쉽게 관리하게 해주는 도구로 사용되기 때문이다. 비동기 기능 자체는 `Promise`로 만든다.

```js
function getId(param) {
  return new Promise(resolve => resolve('abcd'));
}
function getEmail(param) {
  return new Promise(resolve => resolve('abc@d.com'));
}
function getName(param) {
  return new Promise(resolve => resolve('anonymous'));
}
function order(param) {
  return new Promise(resolve => resolve('☕'));
}

function* orderCoffee(phoneNumber) {
  const id = yield getId(phoneNumber);
  const email = yield getEmail(id);
  const name = yield getName(email);
  const result = yield order(name, 'coffee');
  return result;
}

const iterator = orderCoffee('010-1010-1111');
let ret;

(function runNext(val) {
  ret = iterator.next(val);

  if (!ret.done) {
    ret.value.then(runNext);
  } else {
    console.log('done:', ret.value);
  }
})();

// 소스 원본 출처: https://meetup.nhncloud.com/posts/73
```

### async-await의 등장

그러나 async 함수와 `await` 기능이 추가되면서 굳이 제네레이터를 이용하여 비동기 작업을 관리할 필요는 없어졌다.

```js
function getId(param) {
  return new Promise(resolve => resolve('abcd'));
}
function getEmail(param) {
  return new Promise(resolve => resolve('abc@d.com'));
}
function getName(param) {
  return new Promise(resolve => resolve('anonymous'));
}
function order(param) {
  return new Promise(resolve => resolve('☕'));
}

async function orderCoffee(phoneNumber) {
  const id = await getId(phoneNumber);
  const email = await getEmail(id);
  const name = await getName(email);
  const result = await order(name, 'coffee');
  return result;
}

(async function () {
  const result = await orderCoffee('010-1010-1111');
  console.log('result:', result);
})()
```

그렇다고 제네레이터 함수가 아예 쓸모가 없어진 건 아니고 다른 용도로 쓸 대가 있...다는데? 🤷

예를 들어 이런 게 있다:

```js
// 코드 출처: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Iterator
function* fibonacci() {
  let current = 1;
  let next = 1;
  while (true) {
    yield current;
    [current, next] = [next, current + next];
  }
}

const seq = fibonacci();
const firstThreeDigitTerm = seq.find((n) => n >= 100);
```
