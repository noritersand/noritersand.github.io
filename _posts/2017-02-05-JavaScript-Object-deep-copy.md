---
layout: post
date: 2017-02-05 23:45:10 +0900
title: 'JavaScript: Object deep copy'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - deep copy
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)

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

단, 함수는 JSON으로 표현할 수 없기 때문에 복사하지 못한다.
