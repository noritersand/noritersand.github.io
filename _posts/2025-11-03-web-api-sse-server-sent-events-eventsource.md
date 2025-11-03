r---
layout: post
date: 2025-11-03 14:39:19 +0900
title: '[Web API] SSE, Server-sent events'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - html-standard
  - sse
  - server-sent-events
  - eventsource
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Server-sent events - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)
- [Using server-sent events - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)
- [EventSource - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)
- [EventSource: EventSource() constructor - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventSource/EventSource)
- [HTML Standard](https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events)


## 개요

SSE(Server-Sent Events)는 HTTP 연결을 통해 서버가 클라이언트에게 데이터를 실시간으로 단방향 푸시하는 기술이다. 뉴스 피드, 주가 정보, 알림 시스템 등 실시간 업데이트가 필요한 상황에 주로 사용된다.

SSE 통신은 클라이언트의 EventSource 인터페이스를 통해 시작되고, 서버는 연결을 유지한 채 연속적인 스트림으로 응답한다.

ℹ️ SSE는 단방향만 가능하니, 양방향이 필요하면 웹소켓을 쓰자


## 클라이언트측 구현: EventSource

EventSource API로 이벤트를 수신하도록 구현한다.

```
new EventSource(url)
new EventSource(url, options)
```

- `url`: 이벤트나 메시지를 제공하는 원격 자원의 위치. 쉽게 말해서 서버의 SSE 엔드포인트 주소다.
- `options`:
  - `withCredentials`: 기본값 `false`. 동일 출처일 때는 의미 없는 옵션으로, 이 값이 `true` 교차 출처 요청(CORS)일 때도 자격 증명 정보(쿠키, HTTP 인증 헤더, 클라이언트 SSL 인증서 등)를 서버에 전송한다. 서버도 이에 응답하려면 `Access-Control-Allow-Credentials: true` 헤더 필요.

```js
const eventSource = new EventSource('/api/v1/sse');

// 이벤트 유형이 없을 때 실행
eventSource.onmessage = (event) => {
  console.log('받은 데이터:', event.data);
};

// 이벤트 유형이 notice일 때 실행
eventSource.addEventListener('notice', (event) => {
  console.log('알림:', event.data);
});

// 이벤트 유형이 tick일 때 실행
eventSource.addEventListener('tick', (event) => {
  console.log('서버 시각:', event.data);
});
```

```js
// 종료
eventSource.close();
```


## 서버측 구현

서버측의 SSE 응답은 `Content-Type: text/event-stream`, `Connection: keep-alive`, `Cache-Control: no-cache` 헤더를 설정해야 한다.

구현은 대충 아래와 같다:

### JavaScript - Node.js - Express

`res.write()`를 사용하여 응답 스트림을 닫지 않고 데이터를 주기적으로 전송한다:

```js
// Express.js
app.get('/api/sse', (req, res) => {
  // 1. 헤더 설정
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Connection', 'keep-alive');
  res.setHeader('Cache-Control', 'no-cache');

  // ℹ️ withCredentials: true 일 때 필요한 헤더
  // res.setHeader('Access-Control-Allow-Credentials', 'true');
  
  let counter = 0;
  const intervalId = setInterval(() => {
    counter++;
    const time = new Date().toLocaleTimeString();
    
    // 2. SSE 규격 데이터 형식 (id, 이벤트유형(event), data, 종료(\n\n))
    const sseData = `id: ${counter}\nevent: update\ndata: ${time}\n\n`;
    
    // 3. 스트림에 데이터 쓰기
    res.write(sseData);
  }, 1000);

  // 4. 클라이언트 연결 종료 시 정리
  req.on('close', () => {
    clearInterval(intervalId);
    res.end();
  });
});
```

### Java - Spring - WebFlux

Spring WebFlux의 Flux를 사용하여 비동기적으로 이벤트 스트림 처리:

```java
// Spring Boot (WebFlux)
import org.springframework.http.MediaType;
import org.springframework.http.codec.ServerSentEvent;
import reactor.core.publisher.Flux;
import java.time.Duration;

@GetMapping(value = "/api/sse", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<ServerSentEvent<String>> streamEvents() {
    
    // 1. Content-Type은 produces 속성으로 자동 설정됨 (text/event-stream)
    
    // 2. 1초마다 이벤트를 생성하여 스트리밍
    return Flux.interval(Duration.ofSeconds(1))
               .map(sequence -> ServerSentEvent.<String>builder()
                   // 3. SSE 필드를 Builder로 쉽게 작성
                   .id(String.valueOf(sequence))
                   .event("tick")
                   .data(LocalDateTime.now().toString())
                   .build());
}
```

### Python - Flask

yield를 사용하여 제너레이터를 만들고, `Response` 객체에 `text/event-stream` 타입을 지정하여 스트리밍:

```py
# Flask
from flask import Flask, Response
import time

app = Flask(__name__)

# SSE 규격 데이터 형식 함수
def format_sse(data, event=None, id=None, retry=None):
    msg = f'data: {data}\n\n'
    if event:
        msg = f'event: {event}\n{msg}'
    if id:
        msg = f'id: {id}\n{msg}'
    return msg

# SSE 엔드포인트
@app.route('/api/sse')
def stream():
    
    def event_stream():
        count = 0
        while True:
            time.sleep(1) # 1초 대기
            current_time = time.strftime('%H:%M:%S')
            count += 1
            
            # 1. SSE 형식에 맞춰 데이터 생성 후 yield (스트리밍)
            yield format_sse(
                data=f'서버 시간: {current_time}',
                event='tick',
                id=count
            )
            
    # 2. Response 객체로 스트리밍하고 Content-Type 설정
    return Response(
        event_stream(),
        mimetype='text/event-stream'
    )
```
