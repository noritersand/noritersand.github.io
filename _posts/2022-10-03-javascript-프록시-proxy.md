---
layout: post
date: 2022-10-03 21:21:27 +0900
title: '[JavaScript] proxy'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - proxy
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

#### 버전 정보

- x.x.x


## 개요

자바스크립트 Proxy에 대한 간단 정리 글.


## 원본 꺼내기

Proxy는 내부에 숨겨진 프로퍼티 target이 있는데, 원본 객체를 가리킨다. 이걸 바로 꺼내는 방법은 없고 복제만 가능한 것 같다:

```js
var target = {
  message1: "hello",
  message2: "everyone"
};

var handler = {
  get(target, prop, receiver) {
    if (prop === "message2") {
      return "world";
    }
    return Reflect.get(...arguments);
  },
};

// 프록시 생성
var proxy = new Proxy(target, handler);

// 프록시에서 원본 복제
var clone = Object.assign({}, proxy); // Object { message1: "hello", message2: "world" }
```

그런데 엄밀히 따지면 복제라고 할 수도 없다. 원본과 동일한 값이 아니라 프록시 핸들러를 거친 결과가 나오기 때문. 위 코드를 보면 message2의 값이 'everyone'이 아닌 'world'다.


## 곁다리: Vue 3에서...

아래는 Vue 3에서 프록시와 private 멤버를 같이 쓸 수 없는 문제가 발생하길래 테스트해 본 것:

```js
class Abc {
  #value;

  constructor(value) {
    this.#value = value;
  }
  
  of() {
    return this.#value;
  }
}

var obj = {
  abc: new Abc('Hello world!'),
  ov() {
    console.log(this.abc.of()); // Hello world!
  }
}

var handler = {};
var p = new Proxy(obj, handler);

p.abc; // Object { #value: "Hello world!" }
p.ov(); // Hello world!
```

근데 재연이 안됨. 뭔가 다른가 봄. 원래는 이 에러가 나야 함:

```
Cannot read private member #value from an object whose class did not declare it
```

TODO Vue 3 API인 `isProxy()`, `toRaw()`를 찾아볼 것  
https://stackoverflow.com/questions/51096547/how-to-get-the-target-of-a-javascript-proxy
