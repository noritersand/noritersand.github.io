---
layout: post
date: 2019-06-27 17:38:00 +0900
title: '[JavaScript] async function과 await'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - async
  - async-function
  - await
  - promise
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: async function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: await](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await)
- [MDN: Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

#### 기능 정의

- [ECMA-262](https://tc39.es/ecma262/#sec-async-function-definitions)

`async function`과 `Promise` 함수의 결과를 기다리는 `await`를 정리한 글.

## async function

```
async function name() {
  ...
}
```

```
var fn = async function() {
  ...
};
```

`async` 키워드를 사용해 선언하는 함수. 비동기적으로 실행되며, 함수가 실제로 어떤 값을 반환하는지 여부에 관계없이 항상 `Promise`를 반환한다. 만약 반환값이 `undefined`가 아니라면, 이 값은 `Promise` 객체의 숨겨진 프로퍼티에 저장된다.
TODO: 이거 어떻게 꺼내냐?

## await

```
await expression
```

`await`는 async 함수 내부에서만 사용할 수 있는 연산자로, `Promise`를 기다릴 때 사용한다.

#### example

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

### Promise와 같이 사용하는 예시

[코드 출처](https://stackoverflow.com/questions/951021/what-is-the-javascript-version-of-sleep/39914235#39914235)

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function demo() {
  console.log('Taking a break...');
  await sleep(2000);
  console.log('Two seconds later, showing sleep in a loop...');

  // Sleep in loop
  for (let i = 0; i < 5; i++) {
    if (i === 3) {
      await sleep(2000);
    }
    console.log(i);
  }
}

demo();
```

끗.
