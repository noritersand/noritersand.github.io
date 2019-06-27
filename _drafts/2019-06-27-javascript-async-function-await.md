---
layout: post
date: 2019-06-27 17:38:00 +0900
title: 'JavaScript: async function과 await'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - async
  - async function
  - await
  - promise
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN: async function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: await](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await)
- [MDN: Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

`async`는 async function 선언 시 사용되는 키워드이며,
`await`는 async function 내부에서 사용하는 statement(아마도?),
`Promise`는 생성자 함수다.

## 제목

내용

## example

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

끗.
