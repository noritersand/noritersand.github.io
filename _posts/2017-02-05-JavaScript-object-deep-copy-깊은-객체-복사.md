---
layout: post
date: 2017-02-05 23:45:10 +0900
title: 'JavaScript: Object deep copy 깊은 객체 복사'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - object
  - deep copy
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)

`JSON.stringify()`와 `JSON.parse()`를 이용한 깊은 복사 방법.

```js
var copyme = {
  prop: {
    grandson: {
      txt: 'peek-a-boo!',
      fn: function() {
        console.log('don\'t leave me!')
      }
    }
  },
  txt: 'yo'
};
var newone = JSON.parse(JSON.stringify(copyme));

console.log(newone.prop.grandson.txt); // peek-a-boo!
console.log(typeof newone.prop.grandson.fn == 'undefined'); // true, 함수는 복사 불가
```

단, 이 방법은 프로퍼티가 함수일 경우 복사하지 못한다. (JSON으로는 함수를 표현할 수 없기 때문)
