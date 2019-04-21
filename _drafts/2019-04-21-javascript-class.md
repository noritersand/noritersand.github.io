---
layout: post
date: 2019-04-21 22:29:00 +0900
title: 'JavaScript: class'
categories:
  - etc
tags:
  - tag me
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN: Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)

## class 선언

```js
class Newbie {
  constructor(trait) {
    this._trait = trait;
  }
  get trait() {
    return this._trait || 'know nothing';
  }
  set trait(arg) {
    this._trait = arg;
  }
  levelUp() {
    console.log('I feel stronger.');
    this._trait = 'barely shooting an arrow';
  }
}

let noob = new Newbie();

noob.trait; // know nothing
noob.levelUp(); // I feel stronger.
noob.trait; // barely shooting an arrow
```

## class 표현식

TODO
