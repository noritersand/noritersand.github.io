---
layout: post
date: 2017-02-05 23:45:10 +0900
title: '[JavaScript] js객체 깊은 복사 Object deep copy'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - deep-copy
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [MDN: JSON](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [MDN: Object.assign()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d](https://medium.com/better-programming/3-ways-to-clone-objects-in-javascript-f752d148054d)
- [https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d](https://www.codementor.io/junedlanja/copy-javascript-object-right-way-ohppc777d)
- [https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)
- [jQuery: jQuery.extend()](https://api.jquery.com/jquery.extend/)

`Object.assign()`이 있긴한데 이건 얕은 복사(shallow copy)만 가능.

## JSON 문자열로 바꾼뒤 다시 객체로 변환

#### 브라우저 호환

- IE7 이하 사용 불가

`JSON.stringify()`와 `JSON.parse()`를 이용한 객체 복사 방법.

```js
// 복사 대상
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

// 복사
var newone = JSON.parse(JSON.stringify(copyme));

console.log(newone.child.grandson.txt); // peek-a-boo!
console.log(typeof newone.child.grandson.fn == 'undefined'); // true, 함수는 복사 불가
```

단, 함수는 복사가 안되는 한계가 있다. (JSON에는 function 타입이 없기 때문)

## 꼐속...
