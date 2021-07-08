---
layout: post
date: 2018-03-27 12:50:10 +0900
title: '[JavaScript] DOM Event: unload, beforeunload'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - browser
  - eventhandlers
  - onclose
  - onunload
  - window
---

#### 참고한 문서

- [https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload](https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload)
- [MDN: WindowEventHandlers/onunload](https://developer.mozilla.org/ko/docs/Web/API/WindowEventHandlers/onunload)
- [MDN: WindowEventHandlers/onunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onunload)
- [MDN: Window/unload_event](https://developer.mozilla.org/ko/docs/Web/API/Window/unload_event)
- [MDN: Window/beforeunload_event](https://developer.mozilla.org/ko/docs/Web/API/Window/beforeunload_event)
- [MDN: WindowEventHandlers/onbeforeunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload)

## unload

문서가 언로딩(대충 다른 페이지로 이동 중일 때 쯤) 중일 때 발생하는 이벤트.

```js
window.onunload = function() {}
```

얼럿은 차단되지만 스크립트가 실행되긴 한다. 페이지를 이동하거나 새로고침하거나 브라우저를 끌 때도 작동한다. 실행 시간을 오래 잡아먹는 스크립트라면 결과가 다를 수 있다. 아직 잘 몲.
비슷한 `onclose`가 있지만 지원하지 않는 브라우저가 있다.

## beforeunload

```js
window.beforeunload = function() {}
```

문서가 언로드 되기 직전에 발생하는 이벤트. 이 이벤트를 감지하는 시점에 언로드를 취소할 수 있다.

이 이벤트를 사용하면 사용자가 페이지를 떠날 때 확인 대화 상자를 띄울 수 있다:

```js
window.addEventListener('beforeunload', (event) => {
  // 표준에 따라 기본 동작 방지
  event.preventDefault();
  // Chrome에서는 returnValue 설정이 필요함
  event.returnValue = '';
});
```

MDN에 따르면 모든 브라우저에서 이 방법이 통하는 것은 아니라고 한다.
