---
layout: post
date: 2018-03-20 17:15:34 +0900
title: 'JavaScript: Window.postMessage 교차 출처간 메시지 통신'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - windows
  - postMessage
---

#### 참고한 글
- [https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)

#### 브라우저 호환
IE는 11부터 완전히 지원하고 IE8, IE9는 프레임에서만, IE10은 제한적으로만 지원한다. 그 외 브라우저는 모두 가능한걸로... ~~악의축~~

자바스크립트에서는 어떤 윈도우와 또 다른 윈도우의 스킴과 호스트명, 그리고 포트번호가 완전히 일치하지 않을때, 두 윈도우간의 상호작용을 차단한다. 이것은 [동일 출처 원칙(same origin policy)](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)의 제약사항이며 출처가 다른 두 윈도우를 교차 출처(cross origin) 상태에 있다고 한다.

교차 출처인 윈도우의 변수나 함수 즉, 프로퍼티로의 접근은 대부분 차단된다고 생각하면 된다. 가령 A 윈도우에서 교차 출처인 B 윈도우의 주소를 Location을 통해 가져오려고 하면 DOMException이 발생하면서 스크립트가 중단되는 식이다. 참고로 교차 출처 상태일 때 location으로 주소를 알아낼 수는 없지만 주소를 바꿀 수는 있다.

하여간 이런 동일 출처 원칙의 제약을 우회할 수 있는게(비록 엄격한 조건 하에서만 가능하지만) 바로 Window.postMessage다.
```
otherWindow.postMessage(message, targetOrigin, [transfer]);
```
- **message**: 다른 윈도우에 전달할 메시지
- **targetOrigin**: 메시지를 전달할 윈도우의 출처를 명시한다. 대상 윈도우의 스킴, 호스트명, 포트번호가 targetOrigin에 명시된 것과 정확히 일치하지 않으면 메시지는 차단된다.
- **transfer**: Is a sequence of Transferable objects that are transferred with the message. The ownership of these objects is given to the destination side and they are no longer usable on the sending side.

아래는 교차 출처인 A 윈도우 `http://tistory.com`과 B 윈도우 `https://secure.tistory.com`이 서로 메시지를 주고받는 코드다.

```js
// B 윈도우에서 핸들러 설정
function receiveBeginningMessage(event) {
  if (event.origin !== "http://tistory.com") {
    return;
  }
  console.log(event.data); // A 윈도우에서 보낸 메시지
  event.source.postMessage("my location is " + location.href, event.origin);
}
window.addEventListener("message", receiveBeginningMessage);
```
```js
// A 윈도우에서 핸들러 설정
function receiveReturnMessage(event) {
  console.log(event.data); // B 윈도우에서 보낸 메시지
}
window.addEventListener("message", receiveReturnMessage);
```
```js
// A 윈도우에서 Window.postMessage 사용
var child = frames[0];
child.postMessage("whispering", "https://secure.tistory.com");
```
순서는 대략:
- 양쪽 윈도우에 메시지 이벤트 핸들러 등록
- A 윈도우에서 Window.postMessage 호출
- B 윈도우에서 메시지 이벤트가 발생하고 핸들러 작동. 이 때 로그로 A 윈도우가 보낸 message를 출력하고 A 윈도우로 메시지 이벤트를 발생시킴
- A 윈도우에서 메시지 이벤트 핸들러 작동하며 B 윈도우가 보낸 메시지 출력
