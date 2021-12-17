---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[Node.js] 기본 내장 모듈 Built-in Modules'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - fundamental-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- http://www.nodejs.org/api
- http://nodejs.sideeffect.kr/docs

## Assert

유닛 테스트 기능 제공.

## Buffers

바이너리 데이터의 옥텟 스트림(octet stream) 지원 모듈.

## Child Processes

자식(외부) 프로세스 생성/제어.

#### Node.js 에서 콘솔명령어 실행

```
child_process.exec(command, [options], callback)
```

```js
var child = require('child_process');

function dir() {
  var content = 'empty';
  child.exec('ls -lah', function(err, stdout, stderr) {
    content = stdout;
      if (err !== null) {
        content = 'exec error: ' + err;
      }
    console.log(content);
  });
}
dir();
```

## Cluster

클러스터링(다수의 프로세스)을 제공하는 모듈.

## Crypto

암호화 함수 제공.

## DNS

DNS(Domain Name Server) 함수 제공 모듈.

## Events

이벤트 관련.

## File System

파일 제어 모듈.

#### 파일 읽기

```
fs.readFile(filename, [options], callback)
```

```js
var fs = require('fs');

function read(target) {
  fs.readFile('./' + target, encoding='utf8', function(err, data) {
    if (err) {
      throw err;
    }
    console.log('세 번째 로그');
    console.log(data);
  });
  console.log('첫 번째 로그');
};

read('index.js');
console.log('두 번째 로그');
```

이벤트 루프 방식의 처리에 주의할 것.

입출력 관련 모듈이다보니 비동기함수와 동기함수를 모두 API로 제공하는데 동기함수는 보통 비동기함수 + Sync가 이름이다. (e.g. `fs.readFileSync()`)

## 파일 쓰기

```js
fs.writeFile('./log/requestheaders.txt', util.inspect(req.headers), function(err) {
  if (err) {
    throw err;
  }
  console.log('write complete');
});
```

append(이어쓰기)는 `fs.appendFile()` 함수를 사용한다.

## HTTP

HTTP 서버/클라이언트 제어 모듈.

## HTTPS

HTTPS 보안 프로토콜 관련 모듈.

## Net

비동기 네트워크 통신 지원 모듈.

## Os

운영체제 관련 함수 제공 모듈.

## Path

파일 경로 지원 모듈.

## Process

프로세스 제어 모듈. 전역 객체라서 로드 없이 접근한다.

## Query Strings

URL의 쿼리 문자열을 다룬다.

## Readline

스트림 라인 단위 읽기 관련 모듈.

## Streams

스트림 제어 모듈.

## TLS/SSL

TLS/SSL 지원 함수.

## TTY

콘솔 관련 기능 제공.

## UDP/Datagram Sockets

UDP의 데이터그램 소켓 통신 관련 모듈

## URL

url 관련 모듈.

#### request에서 URL Path 가져오기

```js
var http = require('http');
var url = require('url');

http.createServer(function(request, response) {
  var path = url.parse(request.url).pathname;
  console.log(path);
  //console.log(request.url);
  response.end();
}).listen(3000);
```

## Utilities

기능함수를 제공하는 모듈

#### 객체의 문자열 표현을 반환

```js
console.log(require('util').inspect(require('http')));
// http 모듈의 대략적인 구성을 볼 수 있음.

var util = require('util');
console.log(util.inspect(util, { showHidden: true, depth: null }));
// util 객체의 모든 프로퍼티를 검사
```

## Vm

자바스크립트의 컴파일/실행을 지원한다.

## Zlib

zlib 압축 함수 제공
