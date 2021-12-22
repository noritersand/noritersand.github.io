---
layout: post
date: 1970-01-02 00:00:00 +0900
title: '[Node.js] Global Objects'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - global-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- http://www.nodejs.org/api/globals.html
- http://nodejs.sideeffect.kr/docs

## 글로벌 객체란?

브라우저 자바스크립트의 window처럼 어느 소스(혹은 모듈)에서나 접근가능한 객체를 전역 객체(Global Objects)라고 한다.
예를 들어 console.log() 는 global.console.log() 와 같다.

## process

process 객체를 이용하면 실행된 노드 프로세스의 정보를 조회하거나 이벤트를 등록할 수 있다. (process는 events.EventEmitter의 인스턴스다. 이벤트 목록은 API Doc의 event 항목을 참조할 것)
참고: http://devsw.tistory.com/130

### 이벤트 등록

EventEmitter의 emitter.on()과 같다.

```
process.on(이벤트, 리스너);  // 이벤트를 등록하며 해당 이벤트가 발생했을 때 처리할 리스너를 등록한다. 여기서 리스너는 보통 콜백함수다.
process.addListener(이벤트, 리스너);  // on과 같음
process.once(이벤트, 리스너);  // on과 같으나 해당 이벤트에 한 번만 대응하는 리스너를 등록한다.
```

```js
process.on('exit', function(){
  console.log('bye');
});
```

### 스레드 예약

nextTick으로 이벤트 루프의 다음번 루프에 callback을 호출하도록 스레드를 예약할 수 있다. 스레드는 현재 작업을 완료하고 다음 이벤트를 처리할 수 있을 때 nextTick으로 등록한 callback을 차례대로 실행한다.

```
process.nextTick(callback);
```

```js
process.nextTick(function(){
  process.stdout.write('wo');
});

process.nextTick(function(){
  process.stdout.write('rld');
});

process.stdout.write('hello ');

// $node processTest.js
// hello world
```

## console

```
console.log([data], [...])
```

라인을 개행하며 출력하는 stdout 함수. (`stdout(표준출력)`: 표준 출력은 프로그램이 출력 데이터를 기록하는 스트림이다.)

```js
console.log('count: %d', count);
```

`printf()` 와 같은 방식으로 문자열 포맷을 지정할 수 있다. 만약 첫 문자열에 포맷객체가 없다면 각 요소에 자동으로 util.inspect를 사용한다.
`util.inspect(object)`: 객체의 문자열 표현을 반환.
참고로 console.log 함수는 다음처럼 정의되어 있다.

```js
console.log = function(d) {
  process.stdout.write(d + '\n');
};
```

## Class: Buffer


## require()


## require.resolve()


## require.cache


## require.extensions


## __filename, __dirname

현재 실행되는 스크립트의 절대경로를 저장하는 전역변수.
디렉터리 혹은 파일명을 포함한 디렉터리를 나타내며 API 문서에선 전역 범위가 아닌 각 모듈의 지역범위라고 한다.

```js
pwd
# /d/jstest
cat globaltest.js
# console.log(__filename);
node globaltest.js
# d:\jstest\globaltest.js
```

## module


## exports


## setTimeout(callback, millisec), setInterval(callback, millisec)

millisec 후에 callback을 한 번만 실행(timeout)하거나 반복 실행(interval)한다. millisec는 최대 2,147,483,647(약 24.8일)까지 지정할 수 있다.

```js
setTimeout(function() {
  console.log('world');
}), 2000);
console.log('Hello');
```

## clearTimeout(timeoutID), clearInterval(intervalID)

setTimeout 혹은 setInterval 함수의 리스너를 중단한다.

```js
var interval = setInterval(function(){
  console.log('ssup');
}, 1000);

console.log('what');

setTimeout(function(){
  clearInterval(interval);
}, 1500);
```
