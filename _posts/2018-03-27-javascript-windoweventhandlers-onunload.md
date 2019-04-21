---
layout: post
date: 2018-03-27 12:50:10 +0900
title: 'JavaScript: WindowEventHandlers.onunload'
categories:
  - javascript
tags:
  - browser
  - eventhandlers
  - javascript
  - ecmascript
  - onclose
  - onunload
  - windows
  - todo
---

#### 참고 문서
- [https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload](https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload)
- [https://developer.mozilla.org/ko/docs/Web/API/WindowEventHandlers/onunload](https://developer.mozilla.org/ko/docs/Web/API/WindowEventHandlers/onunload)

```js
window.onunload = funcRef;
```
얼럿은 차단되지만 스크립트는 실행됨을 확인했다. 페이지를 이동하거나 새로고침하거나 브라우저를 끌 때도 작동한다.
비슷한 이름의 `onbeforeunload`와 `onclose`가 있지만 지원하지 않는 브라우저가 있다.
