---
layout: post
date: 2019-04-19 16:16:00 +0900
title: 'JavaScript: UUID 생성 함수'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - 코드모음
  - uuid
---

* Kramdown table of contents
{:toc .toc}

[소스 출처](https://stackoverflow.com/questions/105034/create-guid-uuid-in-javascript)

```js
function uuidv4() {
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
    var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
    return v.toString(16);
  });
}

console.log(uuidv4())
```

[소스 출처](https://stackoverflow.com/questions/105034/create-guid-uuid-in-javascript)

```js
function uuidv4() {
  return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
  )
}

console.log(uuidv4());
```

[소스 출처](/https://zetawiki.com/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8_UUID_%EC%83%9D%EC%84%B1)

```js
function guid() {
  function s4() {
    return ((1 + Math.random()) * 0x10000 | 0).toString(16).substring(1);
  }
  return s4() + s4() + '-' + s4() + '-' + s4() + '-' + s4() + '-' + s4() + s4() + s4();
}

console.log(guid());
```
