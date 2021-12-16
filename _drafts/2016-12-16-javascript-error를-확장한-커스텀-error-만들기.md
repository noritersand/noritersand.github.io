---
layout: post
date: 2016-12-16 15:17:00 +0900
title: '[JavaScript] Error를 확장한 커스텀 Error 만들기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - error
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://stackoverflow.com/questions/1382107/whats-a-good-way-to-extend-error-in-javascript](http://stackoverflow.com/questions/1382107/whats-a-good-way-to-extend-error-in-javascript)
- [\[MDN\] Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)

```js
function MyError(message) {
  this.name = 'MyError';
  this.message = message;
  this.stack = (new Error()).stack;
}
MyError.prototype = new Error;  // <-- remove this if you do not want MyError to be instanceof Error
```
