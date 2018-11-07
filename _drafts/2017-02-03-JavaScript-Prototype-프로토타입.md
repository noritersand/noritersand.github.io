---
layout: post
date: 2017-02-03 13:44:00 +0900
title: 'JavaScript: Prototype 프로토타입'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - prototype
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [MDN: 상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)

```js
var sayHi = function(word) {
  this.word = word;
  this.getName = function() {
    return this.word;
  };
};

var sayHi = new sayHi('hi');
console.log(sayHi.getName());
```

```js
var sayHi = function(word) {
  this.word = word;
};

sayHi.prototype.getName = function() {
  return this.word;
};

var sayHi = new sayHi('hi');
console.log(sayHi.getName());
```

위 둘의 결과는 같으나 미묘하게 다르다. ~~그게뭔소리여시방~~ 첫 번째 코드는 인스턴스마다 메서드가 생성되고, 두 번째 코드는 인스턴스에 관계없이 프로토타입의 메서드 딱 하나만 생성된다.
