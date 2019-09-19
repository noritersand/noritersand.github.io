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

#### 관련 문서

- [MDN: Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
- [MDN: Using Fetch](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Fetch%EC%9D%98_%EC%82%AC%EC%9A%A9%EB%B2%95)
- [MDN: Fetch basic concepts](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Basic_concepts)
- [MDN: FetchEvent](https://developer.mozilla.org/ko/docs/Web/API/FetchEvent)

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

```js
await fetch("http://127.0.0.1:8080/test/doughnutList", {
    "credentials": "include",
    "headers": {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0",
        "Accept": "text/html, */*; q=0.01",
        "Accept-Language": "ko-KR,ko;q=0.8,en-US;q=0.5,en;q=0.3",
        "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
        "X-Requested-With": "XMLHttpRequest",
        "Pragma": "no-cache",
        "Cache-Control": "no-cache"
    },
    "referrer": "http://127.0.0.1:8080/krispy-doughnut.html",
    "method": "POST",
    "mode": "cors"
});
```
