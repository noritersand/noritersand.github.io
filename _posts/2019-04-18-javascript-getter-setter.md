---
layout: post
date: 2019-04-18 17:50:00 +0900
title: 'JavaScript: getter/setter'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - getter
  - setter
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)

#### 브라우저 호환

- IE9 이상부터 사용 가능

[ECMAScript 5.1](https://www.ecma-international.org/ecma-262/5.1/#sec-11.1.5)에서 최초로 정의된 구문. `get`, `set` 키워드가 붙은 함수는 객체의 프로퍼티처럼 작동한다.

## class 선언에서

```js
class Newbie {
  get trait() {
    return this._trait || 'know nothing';
  }
  set trait(arg) {
    this._trait = arg;
  }
}

let noob = new Newbie();

noob.trait; // "know nothing"
noob.trait = 'crawl';
noob.trait; // "crawl"
```

## 객체 리터럴에서

```js
var obj = {
  _findMe: 'Hello',
  get findMe() {
    // return this._findMe;
    return 'Nope';
  },
  set findMe(value) {
    // this._findMe = value;
    console.log('Denied');
  }     
};

obj._findMe; // "Hello"
obj.findMe; // "Nope"
obj.findMe = 1234; // Denied
obj.findMe; // "Nope"
```
