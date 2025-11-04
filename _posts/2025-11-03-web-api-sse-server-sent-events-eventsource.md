---
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

SSE(Server-Sent Events)는 HTTP 연결을 통해 서버가 클라이언트에게 데이터를 실시간으로 단방향 푸시하는 기술이다. 뉴스 피드, 주가 정보, 알림 시스템 등 실시간 업데이트가 필요한 상황에 주로 사용된다. SSE 통신은 클라이언트의 EventSource 인터페이스를 통해 시작되고, 서버는 연결을 유지한 채 연속적인 스트림으로 응답한다.

ℹ️ SSE는 단방향만 가능하니, 양방향이 필요하면 웹소켓을 쓰자

🚨 CSRF 공격에 취약하다는 말이 있으니 인증 등에 사용할 때 주의할 것


## 클라이언트 측 구현: EventSource

EventSource API로 이벤트를 수신하도록 구현한다.

⚠️ EventSource는 표준 사양에 따라 HTTP GET 메서드만 지원한다.

```
new EventSource(url)
new EventSource(url, options)
```

- `url`: 이벤트나 메시지를 제공하는 원격 자원의 위치. 쉽게 말해서 서버의 SSE 엔드포인트 주소다.
- `options`:
  - `withCredentials`: 기본값은 `false`. 동일 출처일 땐 있으나 없으나 동일하게 작동하는 옵션. 이 값이 `true`면 교차 출처 요청(CORS)일 때도 자격 증명 정보(쿠키, HTTP 인증 헤더, 클라이언트 SSL 인증서 등)를 서버에 전송한다. 서버도 이에 응답하려면 `Access-Control-Allow-Credentials: true`와 `Access-Control-Allow-Origin: <도메인>` 헤더가 모두 필요하다(`Access-Control-Allow-Origin: *`는 `Access-Control-Allow-Credentials: true`와 함께 사용할 수 없음).

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


## 서버 측 구현

서버 측의 SSE 응답은 `Content-Type: text/event-stream`, `Connection: keep-alive`, `Cache-Control: no-cache` 헤더를 설정해야 한다. 

ℹ️ HTTP/1.1에서는 `Connection: keep-alive`가 필요하지만, HTTP/2 이상에서는 자동으로 연결이 유지되므로 생략 가능

응답 시 데이터는 이런 모양이어야 한다:

```
id: awesomeId\n
event: notice\n
data: 깜짝 이벤트 알림!\n\n
```

- 각 항목의 구분은 줄 바꿈(`\n`)으로 하며, 종료 문자는 두 번의 줄 바꿈(`\n\n`)이다.
- 콜론 뒤의 첫 공백은 무시된다.
- `id`는 필요 없으면 생략할 수 있다.
- `event`는 생략하면 `eventSource.onmessage()` 에서 처리함
- `data`는 생략하면 EventSource API가 제대로 작동하지 않을 수 있다. 빈 값이라도 보내는 걸 권장
- 추가 필드로 `retry`를 지정하면 재연결 지연 시간(밀리초)을 설정할 수 있고, `:`로 시작하는 줄은 주석으로 취급된다.

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
    
    /*
    2. SSE 규격 데이터
    */
    const sseData = `id: ${counter}\nevent: tick\ndata: ${time}\n\n`;
    
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

ℹ️ Nginx를 리버스 프록시로 사용하는 경우 `proxy_buffering off;`와 `X-Accel-Buffering: no`를 설정해야 한다. 그렇지 않으면 이벤트가 버퍼에 쌓였다가 한꺼번에 전송된다.


### Java - Spring - WebFlux

Spring WebFlux의 Flux를 사용하여 비동기적으로 이벤트 스트림 처리:

```java
// Spring Boot (WebFlux)
import org.springframework.http.CacheControl;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.http.codec.ServerSentEvent;
import reactor.core.publisher.Flux;
import java.time.Duration;
import java.time.LocalDateTime;

@GetMapping(value = "/api/sse", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public ResponseEntity<Flux<ServerSentEvent<String>>> streamEvents() {

    // 1초마다 이벤트를 생성하여 스트리밍
    Flux<ServerSentEvent<String>> eventStream = Flux.interval(Duration.ofSeconds(1))
            .map(sequence -> ServerSentEvent.<String>builder()
                    .id(String.valueOf(sequence))
                    .event("tick")
                    .data(LocalDateTime.now().toString())
                    .build());

    return ResponseEntity.ok()
            .cacheControl(CacheControl.noCache())
            .body(eventStream);
}
```

ℹ️ 클라이언트가 재연결 시 마지막으로 받은 이벤트의 아이디를 `Last-Event-ID` 헤더로 전송하므로, 서버에서 이를 받아 누락된 이벤트를 보낼 수도 있다.

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

ℹ️ Flask 앱을 배포할 때는 `gevent`나 `eventlet` 기반 서버를 사용해야 SSE 스트림이 실시간으로 전송된다. Gunicorn, uWSGI 같은 전통적인 WSGI 서버는 응답을 버퍼링하거나 일정 시간 후 연결을 강제로 끊을 수 있다.


## 활용 예: 2FA 인증

SSE는 페이지 새로고침이나 풀링 없이 다른 장치(모바일 앱 등)의 2단계 인증(2FA) 완료를 감지해 로그인하는 시스템에 활용할 수 있다. 여기서 핵심은 HTTP 연결 응답 객체를 사용자 식별값(로그인 아이디 등)과 매핑하여 서버 메모리로 관리하는 것이다.

### 💡 핵심 원리: 연결 식별 및 서버 푸시

1. 연결 식별 및 저장: 아이디와 비번 입력 단계를 통과하면 클라이언트에서 서버와의 SSE 연결을 생성한다. 서버는 해당 연결의 HTTP 응답 스트림 객체(res 객체)를 사용자 식별값과 매핑하여 저장소(인스턴스를 저장할 수 있어야 함. 보통은 서버 메모리)에 저장한다.
2. 상태 대기: 서버는 우선 '인증 대기' 상태를 알리는 메시지를 보낸다.
3. 2단계 인증 진행: 별도의 장치에서 서버에 2단계 인증이 완료되었음을 알린다.
4. 특정 클라이언트에 푸시: 외부의 2단계 인증이 완료되면, 서버는 저장소에서 해당 인증의 식별값으로 메모리에서 응답 객체를 찾아 '2단계 인증 완료' 메시지를 전송한다.
5. 연결 종료: 메시지 전송 직후 서버와 클라이언트 모두 연결을 명시적으로 종료하여 리소스 낭비를 방지한다.

ℹ️ SSE 연결의 응답 객체는 서버 인스턴스의 메모리에만 존재하므로, 서버를 여러 대(클러스터) 운용할 경우 Redis Pub/Sub, short polling + WebSocket fallback, Firebase Cloud Messaging 등을 이용한 메시지 라우팅이 필요하다.

### 💻 코드 요약

아래는 자바스크립트로 만든 실제 작동하는 코드의 일부분이다.

#### 클라이언트

아이디/비밀번호 인증 단계를 통과했다 치고, 서버와의 SSE 연결을 연다. 그 다음 서버의 메시지를 기다리다가 '2단계 인증 완료' 이벤트 수신 시 로그인을 성공 처리하고 연결을 닫는다.

```js
// 클라이언트: 2FA 대기 시작
const eventSource = new EventSource(`/api/v1/sse/login/${userId}`);

// 초기 접속 권한 확인
eventSource.addEventListener('login', event => {
  if (event.data === 'granted') {
    // 2FA 팝업 창 오픈 등
  }
});

// 최종 2FA 성공 메시지 수신
eventSource.addEventListener('2fa-complete', event => {
  console.log("로그인 성공!");
  eventSource.close(); // 연결 명시적 종료
});
```

#### 서버: SSE 연결 엔드포인트

최초 SSE 연결의 응답 객체를 맵에 저장해 특정 클라이언트와의 통로를 확보한다.

```js
const sseClients = new Map();

app.get('/api/v1/sse/login/:userId', (req, res) => {
  // 필수 헤더 설정
  res.setHeader('Content-Type', 'text/event-stream');
  // ...
  sseClients.set(req.params.userId, res);
  
  // 초기 'granted' 메시지 전송
  const sseData = `event: login\ndata: granted\n\n`;
  res.write(sseData);
  
  req.on('close', () => {
    sseClients.delete(req.params.userId); // 연결 해제 시 반드시 제거
  });
});
```

#### 서버: 2FA 완료 처리 엔드포인트

저장된 응답 객체를 찾아 메시지를 전송하고 연결을 끊는다.

```js
app.post('/api/v1/sse/2fa/complete/:userId', (req, res) => {
  const userId = req.params.userId;
  const targetClient = sseClients.get(userId);

  if (targetClient) {
    // 특정 클라이언트에게 '2fa-complete' 메시지 푸시
    const sseData = `event: 2fa-complete\ndata: ok\n\n`;
    targetClient.write(sseData); 
    
    targetClient.end(); // 서버 측에서 연결 종료
    sseClients.delete(userId);
    
    res.status(200).send("OK");
  }
});
```
