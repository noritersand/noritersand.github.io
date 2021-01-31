---
layout: post
date: 2019-04-18 17:50:00 +0900
title: '[JavaScript] getter/setter'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - getter
  - setter
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)

#### 브라우저 호환

- IE8 이하는 사용 불가

[ECMAScript 5.1](https://www.ecma-international.org/ecma-262/5.1/#sec-11.1.5)에서 최초로 정의된 구문. 프로퍼티처럼 접근할 수 있는 함수를 정의한다.

```
{ get prop() { ... } }
{ get [expression]() { ... } }

{ set prop(val) { . . . } }
{ set [expression](val) { . . . } }
```

getter와 setter는 접근자 프로퍼티<sup>accessor property</sup>에 속하는데, 우리가 일반적으로 사용하는 데이터 프로퍼티<sup>data property</sup>와는 다르게 취급된다. 한 가지 예를 들면 `Object.getOwnPropertyDescriptor()`에서 보이지 않는다.

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

## 재정의 불가능한 프로퍼티

요딴식으로 setter 없이 getter만 정의하면:

```js
class Newbie {
  get name() {
    return 'fresh newbie';
  }
}

let noob = new Newbie();
noob.name; // "fresh newbie";
noob.name = 'spoiled'; // strict 모드일 땐 TypeError: setting getter-only property 'name' 발생함.
noob.name; // "fresh newbie";
```

`name`은 일종의 immutable property가 된다.

## 객체 복사에 포함되지 않음

getter/setter는 enumerable한 프로퍼티가 아니므로, `Object.assign()`이나 전개 구문으로 복사할 수 없다:

```js
var noob = new Newbie();
noob.trait = 'eating';
noob; // Object { _trait: "eating" }

var clone = { ...noob };
clone.trait = 'sleeping';
clone; // Object { _trait: "eating", trait: "sleeping" }

var clone2 = Object.assign({}, noob);
clone2.trait = 'do nothing';
clone2 // Object { _trait: "eating", trait: "do nothing" }
```
