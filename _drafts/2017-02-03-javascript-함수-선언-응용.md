---
layout: post
date: 2017-02-03 13:44:00 +0900
title: '[JavaScript] 함수 선언 응용'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - function
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://insanehong.kr/category/javascript/](http://insanehong.kr/category/javascript/)

```js
var foo = [{
  showA: function() {
    console.log('aaa');
  }, showB: function() {
    console.log('bbb');
  }
}, function() {
  console.log('ccc');
}];

foo.push(function() {
  console.log('ddd');
});

foo[0].showA();  // aaa
foo[0].showB();  // bbb
foo[1]();  // ccc
foo[2]();  // ddd

function fn(index){
  var arry = [
    { dec: '1', value: 'first' },
    { dec: '2', value: 'second'},
    { dec: '3', value: 'third' }
  ];
  console.log(arry[index].dec);
  console.log(arry[index]['value']);
}

fn(0);
//1
//first
```
