---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Event Handling'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - html-standard
  - browser
  - window
  - dispatch
  - eventhandlers
  - eventlistener
  - eventtarget
  - onclose
  - onunload
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [\[MDN\] EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)
- [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
- [https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload](https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload)
- [\[MDN\] WindowEventHandlers.onunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onunload)
- [\[MDN\] Window: unload event](https://developer.mozilla.org/en-US/docs/Web/API/Window/unload_event)
- [\[MDN\] Window: beforeunload event](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event)
- [\[MDN\] WindowEventHandlers.onbeforeunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload)

## 개요

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 이벤트 핸들링 관련 API 정리.

## 이벤트 버블링 event bubbling

이벤트가 발생하면 이벤트가 부모로 전파되는 특성

TODO

## 이벤트 캡처링 event capturing

하위 요소에 이벤트 핸들러가 있는지 확인하는 과정

TODO

## 인스턴스 메서드

### EventTarget.addEventListener()

```
addEventListener(type, listener);
addEventListener(type, listener, options);
addEventListener(type, listener, useCapture);
```

- `type`: 이벤트 종류
- `listener`: 이벤트가 발생하면 실행할 함수. 유일한 인자로 event 객체가 전달된다.
- `useCapture`: 캡처링 사용 여부. 기본값은 `false`
- `options`: 
  - `capture`: TODO `true` 혹은 `false`
  - `once`: `true` 혹은 `false`
  - `passive`: TODO `true` 혹은 `false`
  - `signal`: TODO

```js
<button type="button" id="btn">push me</button>
<script>
  var foo = document.querySelector('#btn');
  foo.addEventListener('click', function(event) {
    alert('who? me?');
  });
</script>
```

### EventTarget.removeEventListener()

```js
sdf
```

- `aa`:

sdf

### EventTarget.dispatchEvent()

DOM 이벤트를 수동으로 발동한다. 이런식으로 발생하는 이벤트를 '인공 이벤트(synthetic events)'라고 한댄다.

```
target.dispatchEvent(event)
```

- `target`: 이벤트를 발생시킬 요소
- `event`: 디스패치될 event 객체

```js
var event = new Event('build');

// 이벤트 리슨.
element.addEventListener('build', function (e) { /* ... */ }, false);

// 이벤트 디스패치.
element.dispatchEvent(event);
```

## HTMLElement.click()

클릭 이벤트를 강제로 발생시키는 메서드.

```js
document.querySelector('#input').click();
```

## 이벤트

### load

TODO

### DOMContentLoaded

jQuery의 ready로 잘 알려진 그 이벤트임.

```js
document.addEventListener('DOMContentLoaded', (event) => console.log('DOM fully loaded'));
```

### pageshow

TODO

### unload

문서가 언로딩(대충 다른 페이지로 이동 중일 때 쯤) 중일 때 발생하는 이벤트.

```js
window.onunload = function() {}
```

얼럿은 차단되지만 스크립트가 실행되긴 한다. 페이지를 이동하거나 새로고침하거나 브라우저를 끌 때도 작동한다. 실행 시간을 오래 잡아먹는 스크립트라면 결과가 다를 수 있다. 아직 잘 몲.
비슷한 `onclose`가 있지만 지원하지 않는 브라우저가 있다.

### beforeunload

```js
window.beforeunload = function() {}
```

문서가 언로드 되기 직전에 발생하는 이벤트. 이 이벤트를 감지하는 시점에 언로드를 취소할 수 있다.

이 이벤트를 사용하면 사용자가 페이지를 떠날 때 확인 대화 상자를 띄울 수 있다:

```js
window.addEventListener('beforeunload', (event) => {
  // 표준에 따라 기본 기능 작동 방지
  event.preventDefault();
  // Chrome에서는 returnValue 설정이 필요함
  event.returnValue = '';
});
```

MDN의 설명에는 모든 브라우저에서 이 방법이 통하는 것은 아니라고 한다.
