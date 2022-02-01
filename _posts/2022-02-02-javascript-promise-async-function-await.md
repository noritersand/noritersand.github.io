---
layout: post
date: 2022-02-02 13:42:00 +0900
title: '[JavaScript] Promise, async function, await'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - promise
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [\[MDN\] Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [https://web.dev/promises/](https://web.dev/promises/)

#### 버전 정보

- Promise
  - ES2015에서 최초 정의
  - Chrome 32, Edge 12, FireFox 29, Opera 19, safari 8 이상에서 지원
  - IE에서 사용 불가
- async function, await
  - ES2017에서 최초 정의
  - Chrome 55, Edge 15, FireFox 52, Opera 42, Safari 10.1 이상에서 지원
  - IE에서 사용 불가

## 개요

ECMAScript의 Promise, async function, await 사용법 정리.

## Promise

Promise는 비동기 작업의 완료(혹은 실패)와 결과 값을 나타내는 객체다. jQuery의 [Deferred Object](https://api.jquery.com/category/deferred-object/)와 비슷하다.

Promise 객체는 요딴 상태 중 하나를 가진다:

- pending: initial state, neither fulfilled nor rejected.
- fulfilled: meaning that the operation was completed successfully.
- rejected: meaning that the operation failed.

상태에 대한 정의는 [여기](https://github.com/domenic/promises-unwrapping/blob/master/docs/states-and-fates.md)에서 확인 가능.

웹 워커에서 사용할 수 있다고 함.

### 기본 설명

```
new Promise( executor )
new Promise( function( resolve, reject ) { ... } )
```

- `resolve`: Promise 객체의 상태를 fulfilled로 변경하고 resolve 메시지를 전달하는 함수
- `reject`: Promise 객체의 상태를 rejected로 변경하고 reject 메시지를 전달하는 함수.

`Promise()` 생성자 함수는 `executor`를 실행하고 Promise 객체를 반환한다.

```
promise.then( onFulfilled, onRejected )
```

- `onFulfilled`: resolve 때 실행할 함수
- `onRejected`: reject 때 실행할 함수

`Promise.prototype.then()`은 파라미터로 resolve 혹은 reject를 처리할 핸들러 함수를 받는다.

생성자 함수와 `then()`은 Promise 객체를 반환한다(나중에 설명할 `catch()`와 `finally()`도 마찬가지). 그래서 메서드 체이닝 패턴으로 작성한다:

```js
let willBeSuccess = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success message(or fulfillment value)');
  }, 1000);
});

willBeSuccess.then((message) => {
  console.log(message); // 1초 후 'success message(or fulfillment value)' 출력
});

let willBeFail = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('rejection reason');
  }, 1500);
});

willBeFail.then(() => {
  console.log('do nothing'); // 실행안됨
}, (reason) => {
  console.log(reason); // 1.5초 후 'rejection reason' 출력
});
```

`then()`의 두 번째 파라미터까지 작성하는 일은 꽤 번거롭고 가독성이 별로다. `onRejected`만 전담하는 `catch()`를 써보자:

```
promise.catch( onRejected )
```

- `onRejected`: reject 때 실행할 함수

`Promise.prototype.catch()`는 reject된 경우에 실행할 함수 하나만 받는다. 내부에서 `obj.then(undefined, onRejected)`를 호출한다고 함.

```js
let willBeFail2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('rejection reason');
  }, 1500);
});

willBeFail2.catch((reason) => {
  console.log(reason); // 1.5초 후 'rejection reason' 출력
});
```

### `catch()` 후의 상태

아래 예시를 보면 Promise 객체의 상태가 `onRejected` 호출 후 `fulfilled`로 바뀐다:

```js
var pr3 = new Promise((resolve, reject) => {
  reject('rejection reason');
})
var pr4 = pr3.then(() => {
  console.log('moo');
})
var pr5 = pr4.catch((message) => {
  console.log('ya');
});
var pr6 = pr5.then(() => {
  console.log('ho');
});

console.log(pr3); // Promise { <state>: "rejected", <reason>: "rejection reason" }
console.log(pr4); // Promise { <state>: "rejected", <reason>: "rejection reason" }
console.log(pr5); // Promise { <state>: "fulfilled", <value>: undefined }
console.log(pr6); // Promise { <state>: "fulfilled", <value>: undefined }
```

**주의: 각 메서드가 반환하는 객체는 다 다른 인스턴스**

### 에러 처리

메서드 체인 상의 에러는 `catch()`가 받아준다:

```js
try {
  new Promise((resolve, reject) => {
    resolve();
  }).then((msg) => {
    throw new Error('I am error'); // 얘를 두 줄 위로 올려도 결과는 같음
    console.log('moo');
  }).catch((reason) => {
    console.log('ya');
  }).then((msg) => {
    console.log('ho');
  });
} catch (e) {
  console.error('Are you error?')
}
// catch에서 'ya' 출력
// 두 번째 then에서 'ho' 출력
```

만약 `catch()`가 없으면?

```js
try {
  new Promise((resolve, reject) => {
    throw new Error('I am error');
    resolve();
  }).then((msg) => {
    console.log('moo ya ho');
  });
} catch (e) {
  console.error('Are you error?')
}
// Uncaught (in promise) Error: I am error
```

'I am error'만 출력되는데 아무래도 Promise 내부에 try-catch가 있다고 봐야할 것 같음.

### Promise도 finally가 있다

```
promise.finally( onFinally )
```

`Promise.prototype.finally()`는 Promise가 이행만 되면 resolve/reject에 상관없이 무조건 실행하는 함수를 받는다. `onFinally`는 인자도 없고 Promise의 결과 값에 영향을 주지도 않는다:

```js
var pr7 = new Promise((resolve, reject) => {
  resolve('me is result value');
}).finally(() => {
  console.log('알파카로 만든 파카는 알파카파카');
});
```

`finally()`는 척 노리스처럼 강력해서 에러도 신경쓰지 않는다:

```js
new Promise((resolve, reject) => {
  throw new Error('What?');
}).finally(() => {
  console.log('엄마랑 아들이 택견을 하면 모자이크');
});
// '엄마랑 아들이 택견을 하면 모자이크' 출력됨
```

### `setTimeout()`을 Promise로 감싸기

비동기 함수인 주제에 태고부터 존재했단 이유로 promise를 반환하지 않는 건방진 API를 감싸는 방법이다:

```js
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait(1000)
    .then(() => { console.log('a second has passed') })
    .catch(() => { console.log('something went wrong') });
```

[원 소스 출처](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises#creating_a_promise_around_an_old_callback_api)

## async function

TODO

### sleep \#2

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
    if (i === 3)
      await sleep(2000);
    console.log(i);
  }
}

demo();
```

## await

TODO
