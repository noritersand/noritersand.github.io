---
layout: post
date: 2017-02-05 23:45:10 +0900
title: '[JavaScript] 객체 복제(복사) Object Cloning'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - deep-copy
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [JSON \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Object.assign() \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d](https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d)
- [https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d](https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d)
- [https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)
- [jQuery: jQuery.extend()](https://api.jquery.com/jquery.extend/)


## 개요

JavaScript의 객체 복제에 대한 정리글.

객체 복제(object cloning)보단 객체 복사(object copying)란 말이 더 흔히 쓰이지만 더 맞는 표현인 복제로 명시함.


## 얕은 복제 Shallow Cloning

얕은 복제로 분류한 이유는 객체 안의 다른 객체가 원본 그대로 유지되기 때문:

```js
var child = {
  text: 'Hello World!'
};
var parent = {
  c: child
};
var newObj = {...parent};

newObj === parent // false: 새 인스턴스가 만들어 졌지만
newObj.c === parent.c // true: 프로퍼티가 가리키는 객체까지 복제된 건 아님
```

### 전개 구문

얕은 복제 중에는 가장 간단한 방법이다. 우측의 객체(혹은 프로퍼티)가 좌측으로 병합되는 것에 주의할 것

```js
var obj = {foo: 'bar', x: 42};

var clone1 = {...obj};
clone1; // Object { foo: "bar", x: 42 }
clone1 === obj; // false

var clone2 = {...obj, x: '사십이', c: 1};
clone2; // Object { foo: "bar", x: "사십이", c: 1 }
```

```js
var arr = ['a', 'b', ['c', 'd']];

var clone3 = [...arr];
clone3; // Array(3) [ "a", "b", (2) […] ]
clone3 === arr; // false

var clone4 = [...arr[2]];
clone4; // Array [ "c", "d" ]
```

### Object.assign()

```js
var obj = {a: 1, b: 2};
var clone = Object.assign({}, obj);
clone; // Object { a: 1, b: 2 }

var clone2 = Object.assign({}, obj, {b: 3}); // 객체의 사본을 만들면서 일부 프로퍼티는 재할당
clone2; // Object { a: 1, b: 3 }
```

### JSON 문자열로 바꾼뒤 다시 객체로 변환

`JSON.stringify()`와 `JSON.parse()`를 이용한 얕은 복제 방법. JSON 표현이 불가능한 타입(e.g., Function)은 복제할 수 없다. 일부 타입들을 제외하면 어쨋든 객체간 연결은 끊어지므로 깊은 복제로 분류하는 경우도 있다.

```js
// 복제할 대상
var copyme = {
  child: {
    grandson: {
      txt: 'peek-a-boo!',
      fn: function() {
        console.log('don\'t leave me!')
      }
    }
  },
  txt: 'yo'
};

// 복제본
var newone = JSON.parse(JSON.stringify(copyme));

console.log(newone.child.grandson.txt); // peek-a-boo!
console.log(typeof newone.child.grandson.fn); // undefined, 함수는 복제 불가
```


## 깊은 복제 Deep Cloning

간단한 방법은 없고 따로 정의해야 함.

### #1

⚠️ 이 함수는 getter/setter 메서드가 필드로 바뀌는 하자가 있다:

```js
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  let clone = Array.isArray(obj) ? [] : {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key]);
    }
  }
  return clone;
}
```

테스트 코드:

```js
var child = {
  _x: 3,
  set x(value) {
    this._x = value;
  },
  get x() {
    return 65536;
  }
}

var clone = deepClone(child);
console.log(clone); // Object { _x: 123, x: 65536 }
console.log(child); // Object { _x: 123, x: Getter & Setter }

Object.keys(child); // Array [ "_x", "x" ]
child['x']; // 65536
```

### #2

**TODO** `Object.getOwnPropertyDescriptors()`를 활용해야 할 것 같은데?
