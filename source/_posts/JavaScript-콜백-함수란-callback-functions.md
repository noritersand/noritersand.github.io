---
title: 'JavaScript: 콜백 함수란 callback functions'
date: 2018-03-20 16:54:14
categories:
  - javascript
tags:
  - javascript
  - callback
---

자바스크립트의 함수(function)는 하나의 완성된 객체다. 클래스에 종속적이며 클래스 없이는 접근이 불가능한 자바의 메서드(method)와는 다르다. 자바의 메서드가 단지 인스턴스화 될 클래스의 템플릿으로 존재하는 반면, 함수는 독립적인 객체로 존재하기 때문에 함수만으로 접근이 가능하고 개별선언도 가능하다.
하여간 이러한 특징 덕분에 자바에서는 상상도 못할일이 자바스크립트에선 가능한데... 그건 바로 파라미터로 함수 객체를 전달하는 것.

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

다음의 더욱 해괴한 코드를 보면:
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
③ execute ( ①익명함수, ②파라미터 );
```
3을 호출하는데 1을 첫번째 파라미터로 넘겨준다. 그런데 여기서 첫번째 파라미터는 함수 리터럴, 함수의 정의부분이다. 즉, 뭔가 작동을 해야할 몸체는 아직 아무것도 없다. 그리고 두번째 파라미터 2를 넘겨준다. `execute`는 전달받은 첫번째 파라미터를 실행하는데 그것은 함수다. 그리고 두번째 파라미터를, 첫번째 파라미터(함수객체)의  매개변수로 전달한다.
```js
var http = require("http");
http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```
위에서 두번째로 예를 든 코드와 다를바가 없다. 아주 약간 복잡한 함수가 api로 제공될 뿐이다.
createServer()는 첫 번째 파라미터로 함수를 받는다. 그리고 `createServer()` 메서드가 리턴한 객체의 `listen()` 함수로 http 리스너를 등록한다. 그리곤 request 이벤트가 발생할때마다 첫 번째 파라미터인 리터럴로 작성한 함수가 실행된다.
