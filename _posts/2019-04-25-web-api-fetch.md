---
layout: post
date: 2019-04-25 10:46:00 +0900
title: '[Web API] Fetch'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - fetch-standard
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN \| fetch()](https://developer.mozilla.org/en-US/docs/Web/API/fetch)
- [MDN \| Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN \| Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN \| Fetch basic concepts](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Basic_concepts)
- [MDN \| FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- [Fetch Living Standard](https://fetch.spec.whatwg.org/)
- [MDN \| Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers)
- [MDN \| Request](https://developer.mozilla.org/en-US/docs/Web/API/Request)
- [MDN \| Response](https://developer.mozilla.org/en-US/docs/Web/API/Response)

#### 브라우저 호환

- IE, 사파리 사용 불가


## 개요

fetch는 xhr의 모던한 객체임. 멋-쪄


## fetch()

```
fetch(resource)
fetch(resource, options)
```

대충 간단한 사용법은 아래와 같다:

```js
let options = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json; charset=UTF-8',
  },
  body: JSON.stringify({
    foo: 'bar',
    numeric: 1234567890
  })
};

fetch('/return-my-request-body.data', options).then(resp => {
  console.log('first then:', resp); // Response { type: "basic", status: 200, ok: true, statusText: "OK", ... }

  resp.text().then((text) => {
    console.log('second then', text); // {"foo":"bar","numeric":1234567890}
  });
});
```

`fetch()`의 반환값이 Promise인 건 그럴 수 있는데, `text()` 까지 Promise인 건 특이하다.

`await` 방식으로 바꾸면 아래와 같은 모양이 된다:

```js
let response = await fetch('/get-my-request-body.data', init);
let json = await response.json();
console.log(json);
```


## Headers

**TODO**

곁다리: `FormData`로 파일을 전송할 땐 `Content-Type` 헤더를 설정하지 않아도 `multipart/form-data`로 설정된다. 오히려 명시하면 파일이 전송되지 않을 수 있음.


## Request

**TODO**


## Response

**TODO**

### Response.prototype.text()

https://developer.mozilla.org/en-US/docs/Web/API/Response/text

응답값을 문자열로 반환하는 메서드. 이 메서드의 실제 반환값은 Promise 객체이며, 이 객체의 이행 값이 응답값이다.

### Response.prototype.json()

https://developer.mozilla.org/en-US/docs/Web/API/Response/json

`Response.prototype.text()` 메서드와 비슷한데, 이 쪽은 응답값이 문자열 대신 자바스크립트 객체로 반환된다는 점이 다르다. 실제 반환값이 Promise 객체라는 것은 동일하다.

### Response.prototype.blob()

**TODO**


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
