---
layout: post
date: 2017-08-23 17:02:00 +0900
title: 'JavaScript: Function.prototype'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - function
  - prototype
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function)
- [https://stackoverflow.com/questions/38736902/settimeout-bind-and-this](https://stackoverflow.com/questions/38736902/settimeout-bind-and-this)

## Function.prototype.bind()

`bind()`는 this 등의 인수를 변경한 복제된 함수를 반환하는 메서드다. 아래는 MDN의 한글 설명이다:

> bind() 메서드는 호출될 때 그 this 키워드를 제공된 값으로 설정하고 새로운 함수가 호출될 때 제공되는 주어진 순서의 선행 인수가 있는 새로운 함수를 생성합니다.

~~이게도데체뭔소리여~~

아래는 스택오버플로우에서 본 예:

```java
function LateBloomer() {
  console.log(this.constructor);
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// Declare bloom after a delay of 1 second
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.undefinedbloom = function() {
  window.setTimeout(this.declare, 1000);
};

LateBloomer.prototype.declare = function() {
  console.log(this.constructor);
  console.log('I am a beautiful flower with ' +
this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();
flower.undefinedbloom();
```
