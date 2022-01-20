---
layout: post
date: 2017-02-05 23:45:10 +0900
title: '[JavaScript] 객체 복제(복사) Object Cloning'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - deep-copy
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [\[MDN\] Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d](https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d)
- [https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d](https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d)
- [https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)
- [jQuery: jQuery.extend()](https://api.jquery.com/jquery.extend/)

## 개요

JavaScript의 객체 복제에 대한 정리글.

객체 복제(object cloning)보단 객체 복사(object copying)란 말이 더 흔히 쓰이지만 더 맞는 표현인 복제로 명시함.

## 얕은 복제<sup>shallow cloning</sup>

### Object.assign()

IE에서 사용할 수 없다.

```js
let obj = { a: 1, b: 2 };
let clone = Object.assign({}, obj);
clone; // Object { a: 1, b: 2 }

let clone2 = Object.assign({}, obj, { b: 3 }); // 객체의 사본을 만들면서 일부 프로퍼티는 재할당
clone2; // Object { a: 1, b: 3 }
```

### JSON 문자열로 바꾼뒤 다시 객체로 변환

`JSON.stringify()`와 `JSON.parse()`를 이용한 얕은 복제 방법. IE7 이하에서 사용할 수 없고, JSON으로 표현이 불가능한 함수는 복제할 수 없다. (JSON에는 function 타입이 없기 때문)

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

### 전개 구문

```js
var obj = { foo: 'bar', x: 42 };

var clone1 = { ...obj };
clone1; // Object { foo: "bar", x: 42 }
clone1 === obj; // false

var clone2 = { ...obj, x: '사십이', c: 1 };
clone2; // Object { foo: "bar", x: "사십이", c: 1 }
```

## 깊은 복제<sup>deep cloning</sup>

### 꼐속...
