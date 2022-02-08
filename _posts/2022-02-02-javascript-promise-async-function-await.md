---
layout: post
date: 2022-02-02 13:42:00 +0900
title: '[JavaScript] Promise, async function, await'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - promise
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [\[MDN\] Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [\[web.dev\] JavaScript Promises: 소개](https://web.dev/promises/)
- [\[MDN\] async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [\[MDN\] await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [\[MDN\] Making asynchronous programming easier with async and await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)

#### 버전 정보

Promise:

- ES2015에서 최초 정의
- Chrome 32, Edge 12, FireFox 29, Opera 19, safari 8 이상에서 지원
- IE에서 사용 불가

async function, await:

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

- `resolve`: Promise의 상태를 fulfilled로 변경하고 resolve 메시지를 전달하는 함수
- `reject`: Promise의 상태를 rejected로 변경하고 reject 메시지를 전달하는 함수.

`Promise()` 생성자 함수는 `executor`를 실행하고 Promise 객체를 반환한다.

```
promise.then( onFulfilled, onRejected )
```

- `onFulfilled`: resolve 때 실행할 함수
- `onRejected`: reject 때 실행할 함수

`Promise.prototype.then()`은 파라미터로 resolve 혹은 reject를 처리할 핸들러 함수를 받는다.

생성자 함수와 `.then()`은 Promise를 반환한다(나중에 설명할 `.catch()`와 `.finally()`도 마찬가지). 그래서 메서드 체이닝 패턴으로 작성한다:

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
  console.log('do nothing'); // 실행 안됨
}, (reason) => {
  console.log(reason); // 1.5초 후 'rejection reason' 출력
});
```

`.then()`의 두 번째 파라미터까지 작성하는 일은 꽤 번거롭고 가독성이 별로다. `onRejected`만 전담하는 `.catch()`를 써보자:

```
promise.catch( onRejected )
```

- `onRejected`: reject 때 실행할 함수

`Promise.prototype.catch()`는 reject된 경우에 실행할 함수 하나만 받는다. 내부에서 `promise.then(undefined, onRejected)`를 호출한다고 함.

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

### .catch() 후의 상태

아래 예시를 보면 Promise의 상태가 `onRejected` 호출 후 fulfilled로 바뀐다:

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

메서드 체인 상의 에러는 `.catch()`가 받아준다:

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

만약 `.catch()`가 없으면?

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

### .finally()

```
promise.finally( onFinally )
```

`Promise.prototype.finally()`는 Promise가 이행만 되면 resolve/reject에 상관없이 무조건 실행하는 함수를 받는다. `onFinally`는 인자도 없고 Promise의 결과 값에 영향을 주지도 않는다:

```js
var pr7 = new Promise((resolve, reject) => {
  resolve('me is result value'); // 여기가 reject()여도 결과는 같음
}).finally(() => {
  console.log('알파카로 만든 파카는 알파카파카');
});
// '알파카로 만든 파카는 알파카파카'
```

`.finally()`는 척 노리스처럼 강력해서 에러 따윈 신경쓰지 않는다:

```js
new Promise((resolve, reject) => {
  throw new Error('What?');
}).finally(() => {
  console.log('엄마랑 아들이 택견을 하면 모자이크');
});
// '엄마랑 아들이 택견을 하면 모자이크'
// Uncaught (in promise) Error: What?
```

### setTimeout()을 Promise로 감싸기

비동기 함수인 주제에 태고부터 존재했단 이유로 Promise를 반환하지 않는 건방진 API를 감싸는 방법이다:

```js
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait(1000)
  .then(() => { console.log('a second has passed') })
  .catch(() => { console.log('something went wrong') });
```

[원 소스 출처](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises#creating_a_promise_around_an_old_callback_api)

### Promise.resolve(), Promise.reject()

```
Promise.resolve(value)
Promise.reject(value)
```

각각 `value`가 결과 값이며 상태가 fulfilled 혹은 rejected인 Promise를 반환한다:

```js
Promise.resolve(1).then(console.log); // 1
Promise.reject(2).catch(console.log); // 2
```

## async function

```
async function fn() {}
```

`async` 키워드를 사용해 선언하는 함수. 함수가 실제로 어떤 값을 반환하는지 여부에 관계없이 항상 Promise를 반환한다.

async 함수가 반환한 값은 Promise의 숨겨진 프로퍼티에 저장되기 때문에 꺼내려면 `.then()`이 필요함:

```js
var hello = async () => {
  return 'Hello!';
};
hello().then(console.log); // Hello!
// 이 코드는 아래와 같음
// pr().then((msg) => { console.log(msg); });
```

### Promise 래핑

async 함수의 반환값이 명시적인 Promise가 아니라면 자동으로 Promise로 감싸진다:

```js
async function() {
  return 1;
}
```

이 코드는 아래와 **비슷**하다:

```js
function() {
  return Promise.resolve(1);
}
```

같은게 아니라 비슷한 이유는 async 함수가 Promise로 감싸진 것처럼 작동하지만 완전히 동일하진 않기 때문이다.

만약 반환하려는 참조가 Promise라면 async 함수는 새 참조를 반환하지만 `Promise.resolve()`는 일치하는 참조를 반환한다:

```js
const p = new Promise(res => { res(1) });

async function asyncReturn() {
  return p;
}
console.log(p === asyncReturn()); // false

function basicReturn() {
  return Promise.resolve(p);
}
console.log(p === basicReturn()); // true
```

## await

```
await expression
```

`await`은 async 함수 내부에서만 사용할 수 있는 연산자로, Promise를 기다릴 때 사용한다. 더 정확히 말하자면 `await` 연산자 다음의 Promise가 fulfilled 될 때까지 함수의 실행을 일시 정지시킨다:

```js
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));

var fn = async () => {
  await wait(2000);
  console.log('done');
};
fn();
console.log('아재개그는 아주 재밌는 개그');
// '아재개그는 아주 재밌는 개그'
// 2초 후 'done' 출력
```

### 얘도 래핑을 하네

만약 `await` 다음이 Promise가 아니면 해당 값은 [resolved Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)로 변환된다.

예를 들면 이 코드는:

```js
async () => {
  await 1234;
};
```

다음과 같다:

```js
async () => {
  await Promise.resolve(1234);
};
```

### async 함수를 비동기로 만드는 것은 await

async 함수의 본문은 0개 이상의 `await`으로 분할된다고 볼 수 있다(라고 MDN에서 설명 🥲). 첫 번째 `await`을 만날때 까지 async 함수는 동기적으로 실행된다. 따라서 `await`이 없는 async 함수는 일반 함수처럼 전체가 동기적으로 실행된다:

```js
var fn = async () => {
  console.log(1);
  console.log(2);
};
fn();
console.log('알파카파카파까?');
// 1
// 2
// '알파카파카파까?'
```

하지만 `await`을 만나면 그 부분부터 (async 함수를 호출한 코드의 관점에서) 비동기적으로 작동한다:

```js
var fn2 = async () => {
  console.log('문제: 개성공단의 반대말은?');
  await 1; // 1은 아무 의미 없음
  console.log('🤣🤣🤣');
};
fn2();
console.log('답: 고양이실패단');
// '문제: 개성공단의 반대말은?'
// '답: 고양이실패단'
// '🤣🤣🤣'
```

## Promise의 병렬 처리

### await 사용 시 주의할 점

Promise의 상태 변화를 기다리는 `await`의 동기적 특성은 아주 당연하게도 처리 속도에 악영향을 줄 수 있다:

```js
const wait = ms => new Promise(resolve => setTimeout(resolve, ms));

var timeTest = async () => {
  let startTime = new Date();

  await wait(1000);
  await wait(1000);
  await wait(1000);

  let endTime = new Date();
  console.log(`It takes ${endTime.getTime() - startTime.getTime()} milliseconds.`);
};
timeTest(); // It takes 3028 milliseconds.
```

기다리는 것을 세 번 반복했더니 3초나 걸린다. 이런 경우엔 Promise를 반환하는 표현식 앞에 `await`을 걸지 말고, 일단 모두 실행하도록 한 다음 변수의 상태 변화를 기다리도록 하는게 좋다:

```js
var timeTest2 = async () => {
  let startTime = new Date();

  let result = wait(1000);
  let result2 = wait(1000);
  let result3 = wait(1000);
  await result;
  await result2;
  await result3;

  let endTime = new Date();
  console.log(`done. It takes ${endTime.getTime() - startTime.getTime()} milliseconds.`);
};
timeTest2(); // It takes 1009 milliseconds.
```

거의 1초 정도에 작업이 완료된다.

### Promise.all(), Promise.race()

앞선 예시처럼 변수 여러개에 `await`를 거는 방법은 코드가 예쁘지 않다. Promise가 제공하는 메서드를 써보자.

```
Promise.all(iterable)
```

`Promise.all()`은 여러 Promise의 결과를 집계할 때 사용한다. `iterable`에 Promise 객체 여럿을 배열로 던지면 됨:

```js
const wait2 = ms => new Promise(resolve => setTimeout(() => { resolve(ms) }, ms));

var concurrent2 = async () => {
  return await Promise.all([wait2(3000), wait2(2000), wait2(1000)]);
}
concurrent2().then(console.log);
// 'Array(3) [ 3000, 2000, 1000 ]'
```

가장 빠른놈만 하나 고르는 메서드도 있다.

```
Promise.race(iterable)
```

`Promise.race()`는 주어진 Promise들을 동시에 실행하되 가장 먼저 완료되는 것만 반환한다:

```js
var pick = async () => {
  return await Promise.race([wait2(3000), wait2(2000), wait2(1000)]);
}
pick().then(console.log);
// 1000
```

### 진정한 병렬 처리?

변수에 `await`를 걸든, `Promise.all()`이나 `Promise.race()`를 쓰든 문제가 하나 있다. async 함수가 완료되려면 가장 느린 Promise의 작업이 끝날때까지 기다려야 한다는 것:

```js
var concurrent3 = async () => {
  let startTime = new Date();

  await Promise.all([wait2(5000), wait2(3000), wait2(1000)]).then(console.log);

  let endTime = new Date();
  console.log(`done. It takes ${endTime.getTime() - startTime.getTime()} milliseconds.`);
}
concurrent3();
// done. It takes 5013 milliseconds.
```

가장 느린 Promise인 `wait2(5000)` 때문에 총 실행시간은 약 5초다.

사실 이 문제는 앞선 예시들의 구조 그대로 재사용하긴 힘들고 아래 방법처럼 `.then()`을 각각 호출하는게 대안이 될 수 있다:

```js
var parallel = function() {
  wait2(5000).then(console.log);
  wait2(3000).then(console.log);
  wait2(1000).then(console.log);
}
parallel();
// 1000
// 3000
// 5000
```
