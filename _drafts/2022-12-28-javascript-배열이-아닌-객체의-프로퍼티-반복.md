---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[JavaScript] 배열이 아닌 객체의 프로퍼티 반복'
categories:
  - javascript
tags:
  - tag-me
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)


## 개요

배열이 아닌 object 타입 객체의 프로퍼티를 반복자로 꺼내는 방법을 설명함.

요런 객체가 있다고 가정하고 설명함:

```js


var loopMe = {
  a: 7,
  b: 8,
  c: 9
};

Object.defineProperty(loopMe, 'd', {
  value: 10,
  enumerable: false
});

console.log(loopMe); // Object { a: 7, b: 8, c: 9, d: 10 }
```

## for-in

프로퍼티 설명자에서 `enumerable`이 `true`인 프로퍼티에 한하여 반복할 수 있다.

```js
for (let ele in loopMe) {
  console.log('ele:', ele);
}

// a b c
```

object 타입은 iterable object가 아니라서 for-of는 쓸 수 없다.


## Object 스태틱 메서드

아래 세 개의 메서드를 활용한다. 공통적으로 객체가 소유하면서 열거 가능한 프로퍼티여야 한다는 조건이 있다.

### Object.keys

`Object.keys`는 프로퍼티 이름을 배열로 반환한다.

```js
Object.keys(loopMe); // Array(3) [ "a", "b", "c" ]

Object.keys(loopMe).forEach(key => {
  console.log(loopMe[key]);
}); // 7 8 9
```

### Object.values

`Object.values`는 프로퍼티 값을 배열로 반환한다.

```js
Object.values(loopMe); // Array(3) [ 7, 8, 9 ]
```

### Object.entries

`Object.entries`는 프로퍼티 이름, 프로퍼티 값 pair를 배열로 반환한다.

```js
Object.entries(loopMe); // Array(3) [ Array [ "a", 7 ], Array [ "b", 8 ], Array [ "c", 9 ] ]
```
