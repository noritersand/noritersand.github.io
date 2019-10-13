---
layout: post
date: 2019-10-13 19:32:00 +0900
title: 'Javascript: 빈 객체(empty object)를 판단하는 방법'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - empty object
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [MDN: Object.keys()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
- [https://api.jquery.com/jQuery.isEmptyObject/](https://api.jquery.com/jQuery.isEmptyObject/)
- [https://stackoverflow.com/questions/679915/how-do-i-test-for-an-empty-javascript-object](https://stackoverflow.com/questions/679915/how-do-i-test-for-an-empty-javascript-object)

환경에 따라 여러 방법이 있지만, 여기엔 IE9 부터 사용 가능한 `Object.keys()`만 적는다.

```js
Object.keys({}).length === 0; // true
```
