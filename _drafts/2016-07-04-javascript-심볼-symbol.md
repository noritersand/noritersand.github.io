---
layout: post
date: 2016-07-04 15:59:00 +0900
title: '[JavaScript] 심볼 Symbol'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - symbol
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)


`Symbol`은 원시형(primitive, 기본형) 데이터 타입의 일종으로 ES2015 부터 추가되었다. 다른 기본형 타입인 `Number`, `String`, `Boolean`과 마찬가지로 [래퍼(wrapper) 객체](http://noritersand.tistory.com/536)가 있다.


## 개요

심볼 값, 혹은 심볼이라고 부르는 고유함이 보장되는 값을 반환하는 내장 객체다. 데이터 타입은 `symbol`이다.

`Symbol()` 호출은 매번 고유한 심볼을 반환하는 것이 보장된다. 파라미터가 없거나 있거나 같거나 달라도 반환되는 심볼은 늘 고유하다.

```js
var s = Symbol('qwe');
var s2 = Symbol('qwe');

s !== s2; // true
```


## 스태틱 프로퍼티

### Symbol.iterator

객체가 iterable한지 확인할 때 사용된다:

```js
function isIterable(obj) {
  if (obj == null) {
    return false;
  }
  return typeof obj[Symbol.iterator] === 'function';
}
```

[이런것](https://stackoverflow.com/questions/35949554/invoking-a-function-without-parentheses)도 가능함.


## 스태틱 메서드

### Symbol.for()

```
Symbol.for(key)
```

전역으로 관리되는 심볼 레지스트리에서 `key`에 해당하는 심볼이 있으면 이를 반환한다. 없으면 새로운 심볼을 생성한다. `key`를 기준으로 심볼을 재사용한다는 것만 제외하면 `Symbol()`과 같다.

```js
var s = Symbol.for('qwe');
var s2 = Symbol.for('asd');
var s3 = Symbol.for('asd');

s !== s2; // true
s2 === s3; // true
```
