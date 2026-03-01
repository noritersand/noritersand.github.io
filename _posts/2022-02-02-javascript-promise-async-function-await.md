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

#### 참고 문서

- [Promise \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Using Promises \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [web.dev \| JavaScript Promises: 소개](https://web.dev/promises/)
- [async function \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [await \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [Making asynchronous programming easier with async and await \| MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)

#### 테스트 환경 정보

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

상태에 대한 정의는 [여기](https://github.com/domenic/promises-unwrapping/blob/master/docs/states-and-fates.md)에서 확인.

```
new Promise( executor )
new Promise( function ( resolve, reject ) { ... } )
```

- `resolve`: Promise의 상태를 fulfilled로 변경하고 resolve 메시지를 전달하는 함수. 함수라서 명시적으로 호출해야 함. 이렇게 전달되는 값은 resolved value 혹은 fullfilled value라고도 한다.
- `reject`: Promise의 상태를 rejected로 변경하고 reject 메시지를 전달하는 함수. 이것도 함수다.

`Promise()` 생성자 함수는 `executor`를 실행하고 Promise 객체를 반환한다.

ℹ️ Promise는 웹 워커에서도 사용할 수 있다.

### Promise.prototype.then()

Promise가 이행(fulfilled) 되었을 때 실행되는 메서드. 거부(rejected) 되었을 때 실행될 두 번째 콜백 함수를 받을 수도 있다.

```
promiseInstance.then(onFulfilled)
promiseInstance.then(onFulfilled, onRejected)

function onFulfilled(value) {}
function onRejected(reason) {}
```

- `onFulfilled`: resolve 때 실행할 함수
  - `value`: Promise가 이행되며 전달된 값
- `onRejected`: reject 때 실행할 함수
  - `reason`: Promise가 거부되며 전달된 값

`Promise.prototype.then()`은 첫 번째 파라미터로 resolve를, 두 번째 파라미터로 reject를 처리할 핸들러 함수를 받는다.

생성자 함수와 `.then()`은 Promise를 반환한다(나중에 설명할 `.catch()`와 `.finally()`도 마찬가지). 따라서 메서드 체이닝 패턴으로 작성해야 함:

```js
var willBeSuccess = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success message(or fulfillment value)');
  }, 1000);
});

willBeSuccess.then((message) => {
  console.log(message); // 1초 후 'success message(or fulfillment value)' 출력
});

var willBeFail = new Promise((resolve, reject) => {
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

`.then()` 하나로 예외 처리 코드까지 구현하면 가독성이 별로다. `onRejected`만 전담하는 `.catch()`를 써보자:

### Promise.prototype.catch()

Promise 체인에서 발생하는 모든 에러를 처리하는 메서드.

```
promiseInstance.catch(onRejected)

function onRejected(reason) {}
```

- `onRejected`: reject 때 실행할 함수
  - `reason`: Promise가 거부되며 전달된 값

`Promise.prototype.catch()`는 reject된 경우에 실행할 함수 하나만 받는다. 내부에서 `promise.then(undefined, onRejected)`를 호출한다고 함.

```js
var willBeFail2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('rejection reason');
  }, 1500);
});

willBeFail2.catch((reason) => {
  console.log(reason); // 1.5초 후 'rejection reason' 출력
});
```

#### .catch() 후의 상태

아래 예시를 보면 Promise의 상태가 `onRejected` 호출 후 fulfilled로 바뀐다:

```js
var pr1 = new Promise((resolve, reject) => {
  reject('rejection reason');
});
var pr2 = pr1.then(msg => {
  console.log('pr1-msg:', msg); // 실행 안됨
});
var pr3 = pr2.catch(msg => {
  console.log('pr2-msg:', msg); // pr2-msg: rejection reason
});
var pr4 = pr3.then(msg => {
  console.log('pr3-msg:', msg); // pr3-msg: undefined
});

console.log(pr1); // Promise { <state>: "rejected", <reason>: "rejection reason" }
console.log(pr2); // Promise { <state>: "rejected", <reason>: "rejection reason" }
console.log(pr3); // Promise { <state>: "fulfilled", <value>: undefined }
console.log(pr4); // Promise { <state>: "fulfilled", <value>: undefined }
```

⚠️ `pr1`, `pr2`, `pr3`, `pr4`은 각각 다른 인스턴스이니 주의(매번 새로 만들어짐)

#### Promise 체인 위치에 따른 에러 처리

위에서 모든 에러를 처리한다고 설명했지만, 정확히는 Promise 체인 내에서 `.catch()` 위치를 기준으로 이전에 발생한 에러만 처리한다. 아래 예시를 보자:

```js
new Promise((resolve, reject) => {
  resolve();
  // 여기서 에러가 발생해도 첫 번째 catch()가 처리함
})
  .then(msg => {
    // 첫 번째 then()
    throw new Error('중간 에러'); // 에러 발생
  })
  .catch(error => {
    // 첫 번째 catch()
    // 위에서 발생한 에러 처리
    console.error('에러:', error);
  })
  .then(() => {
    // 두 번째 then()
    // 에러 처리 후에도 계속 이어짐
    console.log('계속 진행');
  })
  .catch(error => {
    // 두 번째 then()에서 발생한 에러 처리
    console.error('에러:', error);
  });
```

`.catch()` 블록에서 에러를 다시 던지지 않으면, 그 이후의 체인에서는 에러가 발생하지 않은 것으로 간주된다. 반대로 아래처럼 에러를 다시 던지면, 그 이후의 `.catch()`가 해당 에러를 받는다:

```js
new Promise((resolve, reject) => {
  throw new Error('에러 발생');
})
  .catch(error => {
    console.error('첫 번째 catch:', error);
    throw error; // 이 에러는 두 번째 catch()에서 처리됨
  })
  .catch(error => {
    // 두 번째 catch()
    console.error('두 번째 catch:', error);
  });
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
  console.error('Are you error?'); // 실행 안됨
}
// Uncaught (in promise) Error: I am error
```

'I am error'만 출력되는데 아무래도 Promise 내부에 try-catch가 있다고 봐야할 것 같음.

### Promise.prototype.finally()

```
promise.finally( onFinally )
```

`Promise.prototype.finally()`는 Promise가 이행만 되면 resolve/reject에 상관없이 무조건 실행하는 함수를 받는다. `onFinally`는 인자도 없고 Promise의 결과 값에 영향을 주지도 않는다:

```js
var pr5 = new Promise((resolve, reject) => {
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

비동기 함수이지만 Promise를 반환하지 않는 API를 감싸는 방법이다:

```js
var wait = ms => new Promise(resolve => setTimeout(resolve, ms));
wait(1000)
  .then(() => {console.log('a second has passed')})
  .catch(() => {console.log('something went wrong')});
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


## async function, await

### async function

```
async function fn() {}
```

`async` 키워드를 사용해 선언하는 함수. 함수가 실제로 어떤 값을 반환하는지 여부에 관계없이 항상 Promise를 반환한다.

`return` 키워드로 반환한 값은 Promise의 숨겨진 프로퍼티에 저장되기 때문에 꺼내려면 `.then()`이나 `await`이 필요하다.

```js
var hello = async () => {
  return 'Hello!';
};
hello().then(console.log); // Hello!
// 아래와 같음
// hello().then((msg) => { 
//   console.log(msg) 
// });
```

### Promise 래핑

async 함수의 반환값이 명시적인 Promise가 아니라면 자동으로 Promise로 감싸진다:

```js
async function () {
  return 1;
}
```

이 코드는 아래와 **비슷**하다:

```js
function () {
  return Promise.resolve(1);
}
```

같은게 아니라 비슷한 이유: async 함수는 항상 새 객체를 만들어서 반환하지만, `Promise.resolve()`는 전달된 값이 Promise일 때 해당 객체를 그대로 반환하기 때문:

```js
var p = new Promise(res => {res(1)});

async function asyncReturn() {
  return p;
}
console.log(p === asyncReturn()); // false

function basicReturn() {
  return Promise.resolve(p);
}
console.log(p === basicReturn()); // true
```

### await

```
await expression
```

`await`은 async 함수 내부에서만 사용할 수 있는 연산자로, Promise를 기다릴 때 사용한다. 더 정확히 말하자면 `await` 연산자 다음의 Promise가 fulfilled 될 때까지 함수의 실행을 일시 정지시킨다:

```js
var wait = ms => new Promise(resolve => setTimeout(resolve, ms));
var fn = async () => {
  await wait(2000);
  console.log('done');
};
fn();
console.log('아재개그는 아주 재밌는 개그');
// '아재개그는 아주 재밌는 개그'
// 2초 후 'done' 출력
```

### await 연산의 결과

`await` 연산자는 표현식을 Promise가 아니라 이행된 값으로 평가되도록 만든다.

다음 예시에서:

```js
(async () => {
  let result = await new Promise(resolve => {
    resolve('abc');
  });
  console.log('result:', result); // 'result: abc'
})();
```

`result`는 Promise가 아니라 `resolve('abc')`에 의해 넘겨진 `abc`다.

### await도 한다! 래핑

만약 `await` 연산자 다음이 Promise가 아니면 해당 값은 [resolved Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)로 변환된다.

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

### async 함수가 아무것도 반환하지 않으면?

async 함수는 명시적인 반환값이 없더라도 Promise 객체를 반환한다. 이 객체는 async 함수의 실행이 종료되었을 때 fulfilled 상태로 바뀐다. 하지만 반환한 값이 없으므로 resolved value는 `undefined`다:

```js
async function waitSomething() {
  await Promise.resolve('done');
}

var p = waitSomething();

console.log(p); // Promise { <state>: "pending" }
p.then(console.log); // undefined
setTimeout(() => console.log(p), 100); // Promise { <state>: "fulfilled", <value>: undefined }
```

### async 함수를 비동기로 만드는 것은 await

async 함수의 본문은 0개 이상의 `await`으로 분할된다고 볼 수 있다(라는 MDN의 설명😇). 첫 번째 `await`을 만날때 까지 async 함수는 동기적으로 실행된다. 이 말은 `await`이 없는 async 함수는 일반 함수와 같다는 말이다:

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

### await의 잘못된 사용

`await`을 적용할 대상은 Promise 객체여야 한다. 만약 문법적 실수로 Promise가 아닌 것을 지정했다면 의도와 다르게 작동하면서 버그가 만들어질 것이다. 아래 예시를 보자:

```js
function promiseMe() {
  return new Promise((resolve) => {
    setTimeout(() => {
      let o = {
        data: 890604
      };
      resolve(o);
    }, 500);
  });
}

async function correctUsage() {
  // correct
  let p = await promiseMe();
  console.log('#1:', p.data);
}

async function incorrectUsage() {
  // wrong
  let data = await promiseMe().data;
  console.log('#2:', data);
}

correctUsage(); // #1: 890604
incorrectUsage(); // #2: undefined
```

문제의 `incorrectUsage()` 함수 내부를 보자.

```js
let data = await promiseMe().data;
```

원래 의도는 Promise의 이행을 기다렸다가 `data`를 꺼내는 것이겠지만 그렇게 작동하지 않는다. 단계별로 나누어 설명하면, 우선 `promiseMe().data`는 `undefined`를 반환한다. `promiseMe()`가 Promise 객체를 반환했으나 `data`라는 프로퍼티가 없으니 `.data`가 `undefined`를 반환하기 때문.

```js
// wrong
let data = await undefined
```

`await` 다음에 위치한 `undefined`는 Promise 객체가 아니니 resolved Promise로 변환된다. 

```js
// wrong
let data = await Promise.resolve(undefined);
```

이제 우변은 `await`에 의해 Promise의 이행값 `undefined`가 된다.

```js
// wrong
let data = undefined;
```

이 문제를 해소하려면, 아래처럼 `await`과 기다릴 대상을 괄호로 묶어줘야 한다:

```js
// correct
let data = (await promiseMe()).data;
```


## Promise의 병렬 처리

### await이 여러 개 있을 때 주의할 점

Promise의 상태 변화를 기다리는 `await`의 동기적 특성은 아주 당연하게도 처리 속도에 악영향을 줄 수 있다:

```js
var wait = ms => new Promise(resolve => setTimeout(resolve, ms));
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
var wait = ms => new Promise(resolve => setTimeout(resolve, ms));
var timeTest2 = async () => {
  let startTime = new Date();

  let result = wait(1000);
  let result2 = wait(1000);
  let result3 = wait(1000);
  await result;
  await result2;
  await result3;

  let endTime = new Date();
  console.log(`It takes ${endTime.getTime() - startTime.getTime()} milliseconds.`);
};
timeTest2(); // It takes 1009 milliseconds.
```

거의 1초 정도에 작업이 완료된다.

### Promise.all(), Promise.race(), Promise.any()

앞선 예시처럼 변수 여러 개에 `await`를 거는 방법은 코드가 예쁘지 않다. Promise가 제공하는 메서드를 써보자.

```
Promise.all(iterable)
```

`Promise.all()`은 여러 Promise의 결과를 집계할 때 사용한다. `iterable`에 Promise 객체 여럿을 배열로 던지면 됨:

```js
var wait = ms => new Promise(resolve => setTimeout(() => {resolve(ms)}, ms));
var concurrent = async () => {
  return await Promise.all([wait(3000), wait(2000), wait(1000)]);
}
concurrent().then(console.log); // 'Array(3) [ 3000, 2000, 1000 ]'
```

가장 빠른놈만 하나 고르는 메서드도 있다.

```
Promise.race(iterable)
```

`Promise.race()`는 주어진 Promise들을 동시에 실행하되 가장 먼저 완료되는 것만 반환한다:

```js
var wait = ms => new Promise(resolve => setTimeout(() => {resolve(ms)}, ms));
var pickOne = async () => {
  return await Promise.race([wait(3000), wait(2000), wait(1000)]);
}
pickOne().then(console.log); // 1000
```

```
Promise.any(iterable)
```

`Promise.any()`는 `race()`와 비슷하지만, 가장 먼저 '성공'한 Promise의 결과를 반환한다. 만약 모두 실패(거부)하면 `AggregateError`를 발생시킨다:

```js
var doRresolve = ms => new Promise(resolve => setTimeout(() => {resolve(ms)}, ms));
var doReject = ms => new Promise((resolve, reject) => setTimeout(() => {reject(ms)}, ms));

(async () => {
  return await Promise.any([doRresolve(3000), doRresolve(2000), doRresolve(1000)]);
})().then(console.log); // 1000

(async () => {
  return await Promise.any([doRresolve(3000), doReject(2000), doReject(1000)]);
})().then(console.log); // 3000

(async () => {
  return await Promise.any([doReject(3000), doReject(2000), doReject(1000)]);
})().then(console.log); // AggregateError: No Promise in Promise.any was resolved
```

### 진짜 병렬 처리?

변수에 `await`를 걸든, `Promise.all()`를 쓰든 문제가 하나 있다. async 함수가 완료되려면 가장 느린 Promise의 작업이 끝날때까지 기다려야 한다는 것:

```js
var wait = ms => new Promise(resolve => setTimeout(() => {resolve(ms)}, ms));
var concurrent = async () => {
  let startTime = new Date();

  await Promise.all([wait(5000), wait(3000), wait(1000)]).then(console.log);

  let endTime = new Date();
  console.log(`It takes ${endTime.getTime() - startTime.getTime()} milliseconds.`);
}
concurrent();
// It takes 5013 milliseconds.
```

가장 느린 Promise인 `wait(5000)` 때문에 총 실행시간은 약 5초다.

`Promise.race()`를 쓰면 가장 빠른 Promise만 기다리면 되긴 하지만, 느린 Promise의 실행 결과를 처리할 수 없다는 문제가 있다.

이 문제는 앞선 예시들의 구조 그대로 재사용하긴 힘들고 아래 방법처럼 `.then()`을 각각 호출하는게 대안이 될 수 있다:

```js
var wait = ms => new Promise(resolve => setTimeout(() => {resolve(ms)}, ms));
var parallel = function () {
  wait(5000).then(console.log);
  wait(3000).then(console.log);
  wait(1000).then(console.log);
}
parallel();
// 1000
// 3000
// 5000
```


## 여담: 메서드 이름 짓기

만약 Promise를 반환하는 걸 메서드 이름으로 표현하기로 했다면 Node.js에서 동기 방식의 메서드에 `Sync`를 붙이는 것과 반대로 `Async` 혹은 `Promise`를 뒤에 덧붙이는 방법이 있다. 

예시: 

- `removeSomethingAsync()`
- `drawGridPromise()`


끝.
