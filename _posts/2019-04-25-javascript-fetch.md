---
layout: post
date: 2019-04-25 10:46:00 +0900
title: '[JavaScript] Fetch'
categories:
  - javascript
tags:
  - javascript
  - fetch-standard
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] fetch()](https://developer.mozilla.org/en-US/docs/Web/API/fetch)
- [\[MDN\] Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [\[MDN\] Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [\[MDN\] Fetch basic concepts](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Basic_concepts)
- [\[MDN\] FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- [Fetch Living Standard](https://fetch.spec.whatwg.org/)
- [\[MDN\] Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers)
- [\[MDN\] Request](https://developer.mozilla.org/en-US/docs/Web/API/Request)
- [\[MDN\] Response](https://developer.mozilla.org/en-US/docs/Web/API/Response)

#### 브라우저 호환

- IE, 사파리 사용 불가


## 개요

fetch는 xhr의 모던한 객체임. 멋-쪄


## fetch()

대충 간단한 사용법은:

```js
let params = {
  foo: 'bar',
  numeric: 1234567890,
  user: 'waldo'
};

let init = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json; charset=UTF-8',
  },
  body: JSON.stringify(params)
};

fetch('/get-my-request-body.data', init).then(
  response => response.text() // .json(), etc.
  // same as function(response) {return response.text();}
).then(text => {
  console.log(text);
});
```

요로케 됨.

`await` 방식으로 바꾸려면 두 번 기다려야 함:

```js
let response = await fetch('/get-my-request-body.data', init);
let json = await response.json();
console.log(json);
```

`fetch()`에서 반환하는 `response`도 promise이기 때문.


## Headers

TODO


## Request

TODO


## Response

TODO


## example

소스 출처: [https://stackoverflow.com/questions/9713058/send-post-data-using-xmlhttprequest](https://stackoverflow.com/questions/9713058/send-post-data-using-xmlhttprequest)

```js
let resp = await fetch("http://127.0.0.1:8080/test/doughnutList", {
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
