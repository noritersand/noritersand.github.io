---
layout: post
date: 2013-08-22 00:00:00 +0900
title: 'Node.js: 요청(request) 정보 분석'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - request
---

```js
var fs = require('fs');
var http = require('http');
var server = http.createServer();
var util = require('util');

/*
 *    server.on('request', callback);
 *     : request 이벤트 리스너를 등록한다. 해당 이벤트가 발생하면 callback 실행
 *   : 여기서 callback은 콜백함수 혹은, 이벤트 핸들러 함수라고 한다.
 *   : 이벤트발생 -> 해당하는 리스너가 있다면 캐치 -> 리스너의 핸들러 실행
 *
 */
server.on('request', function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});  // 200 : 요청이 성공했음을 나타내는 HTTP 상태 코드
    res.write(req.url + '\n' + req.method + '\n');  // 요청 URL(기본값 : /)과 요청 방식(GET|POST|DELETE|HEAD)
    res.end(util.inspect(req.headers));   // 헤더정보
}).listen(4000);

console.log('server started at http://localhost:4000');
```

req.headers 는 객체라서 출력하려면 객체 속성 분석 함수인 util.inspect()를 사용한다.

header 객체의 속성 키는 소문자.
