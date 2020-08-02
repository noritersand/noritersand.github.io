---
layout: post
date: 2019-04-22 09:45:00 +0900
title: '[JavaScript] Factory function'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - factory-function
  - create
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [somewhere](somewhere)

```js
const proto = {
  drive () {
    console.log('Vroom!');
  }
};

function factoryCar () {
  return Object.create(proto);
}

const car3 = factoryCar();
console.log(car3.drive());
```

`Object.create()`로 객체를 만드는 방식. 누군가는 [이게 좋다](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e)고 주장하던데...
