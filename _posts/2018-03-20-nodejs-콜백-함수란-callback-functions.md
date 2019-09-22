---
layout: post
date: 2018-03-20 16:54:14 +0900
title: 'Node.js: 콜백 함수란 callback functions'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - callback
---

자바스크립트의 함수(function)는 하나의 완성된 객체다. 클래스에 종속적이며 클래스 없이는 접근이 불가능한 자바의 메서드(method)와는 다르다. 자바의 메서드가 단지 인스턴스화 될 클래스의 템플릿으로 존재하는 반면, 함수는 독립적인 객체로 존재하기 때문에 함수만으로 접근이 가능하고 개별선언도 가능하다.

이러한 특징 덕분에 파라미터로 함수 자체를 전달하는 것이 가능한데,

요딴 식이다:

```js
function say(word) {
  console.log(word);
}
function execute(someFunction, value) {
  someFunction(value);
}
execute(say, "Hello");
```

코드 출처 - manuel kiessling의 'The Node Beginner Book'

아래의 더욱 해괴한 코드를 보면:

```js
function execute(someFunction, value) {
  someFunction(value);
}
execute(function(word) {
  console.log(word)
}, "Hello");
```

이 코드는 사실 맨 처음 예를 든 코드의 단축형이다.

```
<3>execute ( <1>익명함수, <2>파라미터 );
```

3을 호출하는데 1을 첫번째 파라미터로 넘겨준다. 그런데 여기서 1은 함수 리터럴 혹은 함수를 가리키는 변수다. 즉, 뭔가 작동을 해야할 몸체는 아직 아무것도 없다. 그리고 두번째 파라미터 2를 넘겨준다. `execute`의 바디에선 전달받은 첫번째 파라미터를 실행하는데 이것은 함수다. 그러면서 동시에 두번째 파라미터인 2를 첫번째 파라미터(함수객체)의 파라미터로 전달한다. 파라미터 파라미터 파라미터...

```js
var http = require("http");
http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

위에서 두번째로 예를 든 코드와 크게 다르지 않다. `createServer()`는 첫 번째 파라미터로 함수를 받는다. 그리고 `createServer()` 함수가 리턴한 객체의 `listen()` 함수로 http 리스너를 등록한다. 그리곤 request 이벤트가 발생할때마다 첫 번째 파라미터인 리터럴로 작성한 함수가 실행된다.
