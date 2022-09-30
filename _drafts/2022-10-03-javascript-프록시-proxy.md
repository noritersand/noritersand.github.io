---
layout: post
date: 2022-10-03 21:21:27 +0900
title: '[JavaScript] 프록시'
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

자바스크립트 프록시에 대한 간단 정리 글.

## TODO

Proxy는 내부에 숨겨진 프로퍼티인 target(감싼 객체의 원본)을 갖고 있는데 이걸 받아오는 방법은 없는 것 같고 객체 복제는 가능함.

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

var proxy = new Proxy(target, handler);

Object.assign({}, proxy);
```

아래는 Vue3에서 프록시와 private 멤버를 같이 쓸 수 없는 문제가 발생하길래 테스트해 본 것:

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
