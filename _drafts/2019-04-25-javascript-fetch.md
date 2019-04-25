---
layout: post
date: 2019-04-25 10:46:00 +0900
title: 'JavaScript: Fetch'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - fetch
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN: Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)

#### 브라우저 호환

- IE에서 사용 불가
- TODO

xhr의 모던한 객체임. 멋-쪄

## example

소스 출처: https://stackoverflow.com/questions/9713058/send-post-data-using-xmlhttprequest

```js
const url = "http://example.com";
fetch(url, {
  method : "POST",
  body: new FormData(document.getElementById("inputform")),
  // -- or --
  // body : JSON.stringify({
    // user : document.getElementById('user').value,
    // ...
  // })
}).then(
  response => response.text() // .json(), etc.
  // same as function(response) {return response.text();}
).then(
  html => console.log(html)
);
```
