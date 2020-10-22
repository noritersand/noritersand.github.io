---
layout: post
date: 2020-04-08 12:28:00 +0900
title: '[JavaScript] function*, generator function'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - es2015
  - function*
  - generator
  - generator-function
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: Generator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [MDN: function\*](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
- [https://wonism.github.io/javascript-generator/](https://wonism.github.io/javascript-generator/)

#### 브라우저 호환

- Firefox
- Chrome 39 or higher
- 이 외 브라우저에서 지원 안 함

## generator function

```
function* name([param[, param[, ... param]]]) {
  statements
}
```

generator function은 호출 시 코드를 실행하는 대신 `Generator` 객체를 반환한다. 이후 `Generator.prototype.next()`가 호출되면 그때서야 `yield`를 만날 때까지 실행하며 `yield` 우측의 값을 반환한다.

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
  generate: function* () { }
};
```

ES2015의 [메서드 단축 표기법](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#메서드_정의)으로는 이렇게 쓴다:

```js
var obj = {
  * generate() { }
};
```

## 생성자로 사용 불가

generator function은 (엄격 모드가 아니어도) 생성자로 사용할 수 없다.

```js
function* fn() {}
var gen = new fn; // TypeError: f is not a constructor
gen = new fn(); // TypeError: f is not a constructor
```

## 꼐속...
