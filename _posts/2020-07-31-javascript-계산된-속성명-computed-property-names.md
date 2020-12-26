---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 계산된 프로퍼티명 Computed property names'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - computed-property-names
  - es2015
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer](https://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)

#### 버전 정보

- ES2015에서 소개됨
- Chrome 60/Edge 79/Firefox 55/Opera 47/Safari 11.1 이상에서 사용 가능
- IE에서 사용 불가

프로퍼티의 이름을 정의하는 새로운 방법을 정리한 글.

```
{
  [expression]: value
}
```

객체 리터럴의 프로퍼티명 자리에 대괄호`[]`와 표현식의 조합으로 사용한다. `expression`의 실행 결과가 프로퍼티의 이름이 된다.

```js
var prop = 'bbyong';
var obj = {
  [prop]: 123
};
obj; // Object { bbyong: 123 }
```

```js
var obj = {
  ['ddi' + 'yong']: 456
};
obj; // Object { ddiyong: 456 }
```

```js
var i = 0;
var obj = {
  ['idx' + i++]: i,
  ['idx' + i++]: i,
  ['idx' + i++]: i
};
obj; // Object { idx0: 1, idx1: 2, idx2: 3 }
```

```js
var nm = 'waldo';
var obj = {
  [nm.charAt(0).toUpperCase() + nm.slice(1)]: 'Hello there! Mighty fine morning'
};
obj; // Object { Waldo: "Hello there! Mighty fine morning" }
```

당연히 메서드명 또한 표현식으로 정의할 수 있다:

```js
var obj = {
  ['john' + 'snow']: function() {
    console.log('I know nothing.');
  }
};
obj.johnsnow(); // I know nothing.
```
