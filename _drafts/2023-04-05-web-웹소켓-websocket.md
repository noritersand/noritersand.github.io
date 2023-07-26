---
layout: post
date: 2023-04-05 17:03:28 +0900
title: '[WEB] 웹소켓'
categories:
  - web
tags:
  - web
  - websocket
  - javascript
  - push
  - fetch
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] The WebSocket API (WebSockets)](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
- [\[MDN\] WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

#### 버전 정보

- x.x.x


## 개요

어쩌구


## TODO

```js
// Create WebSocket connection.
const socket = new WebSocket("ws://localhost:8080");

// Connection opened
socket.addEventListener("open", (event) => {
  socket.send("Hello Server!");
});

// Listen for messages
socket.addEventListener("message", (event) => {
  console.log("Message from server ", event.data);
});
```


## 실제 통신의 네트워크 트래픽

### Request

```
GET ws://123.45.78.9:9090/agent/activeThread.pinpointws HTTP/1.1
Host: 123.45.78.9:9090
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Upgrade: websocket
Origin: http://123.45.78.9:9090
Sec-WebSocket-Version: 13
Accept-Encoding: gzip, deflate
Accept-Language: ko,en-US;q=0.9,en;q=0.8
Cookie: _ga=GA1.1.1142912298.1680681568; _gid=GA1.1.1853913813.1680681568; _gat=1
Sec-WebSocket-Key: udisVPuynsH9i6HMWy3beQ==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```

### Response

```
HTTP/1.1 101
Cache-Control: no-store
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Accept: nR+RiV0xFf4o9lRR3SMnuNlNrDI=
Sec-WebSocket-Extensions: permessage-deflate;client_max_window_bits=15
Date: Wed, 05 Apr 2023 08:17:10 GMT
```

### messages

```js
{"type": "PING"}
```


## 실패하는 경우들

### 인프라의 미지원

웹소켓은 WS 프로토콜과 WSS(TLS 버전) 프로토콜을 사용하는데, 네트워크 관련 인프라(혹은 장비)에서 이를 허용하지 않으면 통신 실패가 발생한다.

실제로 현재(2023-04-05) 네이버 클라우드는 웹소켓 프로토콜을 허용하지 않고 있으며, 핀포인트 (최신 버전에 추가된) 웹소켓으로 구현된 기능이 작동하지 않는다:

```
WebSocket connection to 'wss://pinpoint.awesome.url/agent/activeThread.pinpointws' failed
```

네이버 클라우드는 로드 밸런서에서만 지원하지 않는 경우라서 핀포인트 서버에 IP로 직접 접속하면 정상 작동하긴 함.
