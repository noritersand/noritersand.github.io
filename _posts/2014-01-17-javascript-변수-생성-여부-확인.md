---
layout: post
date: 2014-01-17 15:19:00 +0900
title: '[JavaScript] 변수 생성 여부 확인'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - variable
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

## try-catch

```js
var flag;

try {
  foo;
  flag = true;
} catch (e) {
  flag = false;
}

console.log(flag);
```

## typeof

```js
var flag;

if (typeof foo == 'undefined') {
  flag = false;
} else {
  flag = true;
}

console.log(flag);
```

### 응용: 특정 함수나 객체가 생성될 때까지 대기

```js
setTimeout(() => {
  window.abcd = '';
}, 3000);

(function waitUntilDefined() {
  if (typeof abcd === 'undefined') {
    console.debug('not defined');
    setTimeout(waitUntilDefined, 500);
    return;
  }
  console.debug('defined');
})();
```

## in

프로퍼티가 있는지 확인하는 방법. 그러니까 로컬 변수에는 적용할 수 없다.

```js
var abc;
'abc' in window; // true

function fn() {
  var def;
  console.log('def' in window); // false
  console.log('def' in this); // false
  console.log('def' in fn); // false
}
fn();
```
