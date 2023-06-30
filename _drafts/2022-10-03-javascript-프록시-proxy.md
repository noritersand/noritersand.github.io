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


## Proxy 만들기

```
new Proxy(target, handler)
```

- `target`: TODO 설명
- `handler`: TODO 설명

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

Object.getOwnPropertyNames(proxy); // Array [ "message1", "message2" ]
proxy.message1; // "hello"
proxy.message2; // "world" 
```


## 원본 꺼내기

Proxy에 원본 객체를 가리키는 내부의 숨겨진 프로퍼티인 `target`이 존재하긴 하지만, `target`은 직접 접근이 안 되며 Proxy의 원본을 꺼낼 방법은 존재하지 않는다.

대신 얕은 복제본을 얻는 것은 가능하다:

```js
// 프록시가 가리키는 원본 복제 #1
var clone = Object.assign({}, proxy); // Object { message1: "hello", message2: "world" }

// 프록시가 가리키는 원본 복제 #1
var clone2 = { ...proxy }; ; // Object { message1: "hello", message2: "world" }

// 프록시가 가리키는 원본 복제 #3
var clone3 = JSON.parse(JSON.stringify(proxy)); // Object { message1: "hello", message2: "world" }
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

근데 뭔가 다른지 재연이 안됨. 

의도대로라면 요 에러가 발생해야 함:

```
Cannot read private member #value from an object whose class did not declare it
```

TODO Vue 3 API인 `isProxy()`, `toRaw()`를 찾아볼 것  
https://stackoverflow.com/questions/51096547/how-to-get-the-target-of-a-javascript-proxy


## 프로퍼티의 변경 감시

아래 코드는 setter를 이용한 프로퍼티 변경 감시 방법을 구글링한 건데:

```js
var obj = {
  get foo() {
    console.log({name: 'foo', object: obj, type: 'get'});
    return obj._foo;
  },
  set bar(val) {
    console.log({name: 'bar', object: obj, type: 'set', oldValue: obj._bar});
    return obj._bar = val;
  }
};

obj.bar = 2;
// {name: 'bar', object: <obj>, type: 'set', oldValue: undefined}

obj.foo;
// {name: 'foo', object: <obj>, type: 'get'}
```

프록시를 써서 구현하면 이렇게 된다 함:

```js
// 코드 출처: https://stackoverflow.com/questions/36258502/why-has-object-observe-been-deprecated
var obj = {
  foo: 1,
  bar: 2
};

var proxied = new Proxy(obj, {
  get: function(target, prop) {
    console.log({type: 'get', target, prop});
    return Reflect.get(target, prop);
  },
  set: function(target, prop, value) {
    console.log({type: 'set', target, prop, value});
    return Reflect.set(target, prop, value);
  }
});

proxied.bar = 2;
// {type: 'set', target: <obj>, prop: 'bar', value: 2}

proxied.foo;
// {type: 'get', target: <obj>, prop: 'bar'}
```
