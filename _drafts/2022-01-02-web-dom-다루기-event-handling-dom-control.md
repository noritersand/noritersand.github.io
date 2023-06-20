---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[WEB] DOM 다루기: Event Handling'
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


## 개요

내 맴대로 분류한 DOM 이벤트 핸들링 관련 API 정리.


## 이벤트 버블링 event bubbling

이벤트가 발생하면 이벤트가 부모로 전파되는 특성

TODO


## 이벤트 캡처링 event capturing

하위 요소에 이벤트 핸들러가 있는지 확인하는 과정

TODO


## 인스턴스 메서드

### EventTarget.addEventListener()

[https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

```
addEventListener(type, listener);
addEventListener(type, listener, options);
addEventListener(type, listener, useCapture);
```

- `type`: 이벤트 종류
- `listener`: 이벤트가 발생하면 실행할 함수. 유일한 인자로 event 객체가 전달된다.
- `useCapture`: 캡처링 사용 여부. 기본값은 `false`
- `options`: 
  - `capture`: 기본값은 `false`. `true`로 지정되면 DOM tree 하위의 `EventTarget`으로 이벤트가 전달되기 전에 이 리스너가 먼저 발동한다.(.. 라고 하는데 뭔 소리야)
  - `once`: 기본값은 `false`. `true`인 경우 리스너가 발동된 직후 제거된다.
  - `passive`: 기본값은 `false`. 몇몇 브라우저와 특정 이벤트는 기본값이 `true`다.
  - `signal`: TODO

```js
function clickHandler(event) {
  alert('who? me?');
}
element.addEventListener('click', clickHandler);
```

### Passive event listener

TODO passive event listener에 대한 설명 추가

- https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#syntax
- https://medium.com/@Esakkimuthu/passive-event-listeners-5dbb1b011fb1
- https://developer.chrome.com/docs/lighthouse/best-practices/uses-passive-event-listeners/

### EventTarget.removeEventListener()

[https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)

```
removeEventListener(type, listener);
removeEventListener(type, listener, options);
removeEventListener(type, listener, useCapture);
```

- `type`
- `listener`: 제거할 이벤트 핸들러를 지정한다. 생략하면 안지워짐.
- `useCapture`
- `options`
  - `capture`


```js
element.removeEventListener('click', clickHandler);
```

### EventTarget.dispatchEvent()

DOM 이벤트를 수동으로 발동한다. 이런식으로 발생하는 이벤트를 '인공 이벤트(synthetic events)'라고 한댄다.

```
target.dispatchEvent(event)
```

- `target`: 이벤트를 발생시킬 요소
- `event`: 디스패치될 event 객체

```js
var event = new Event('build');

// 이벤트 리슨
element.addEventListener('build', function (e) { /* ... */ }, false);

// 이벤트 발동
element.dispatchEvent(event);
```

### HTMLElement.click()

클릭 이벤트를 강제로 발생시키는 메서드.

```js
document.querySelector('#input').click();
```


## 이벤트 목록

### Window: load event

[https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event)

TODO

### DOMContentLoaded

[https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event)  
[https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event)

TODO ? 외 두 개지 🤔

jQuery의 ready로 잘 알려진 그 이벤트임.

```js
document.addEventListener('DOMContentLoaded', (event) => console.log('DOM fully loaded'));
```

### Window: pageshow event

[https://developer.mozilla.org/en-US/docs/Web/API/Window/pageshow_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/pageshow_event)

TODO

### Window: unload event

[https://developer.mozilla.org/en-US/docs/Web/API/Window/unload_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/unload_event)  
[https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload](https://html.spec.whatwg.org/multipage/webappapis.html#handler-window-onunload)

**사용금지된 이벤트**

문서가 언로딩(대충 다른 페이지로 이동 중일 때 쯤) 중일 때 발생하는 이벤트.

```js
window.onunload = function() {}
```

얼럿은 차단되지만 스크립트가 실행되긴 한다. 페이지를 이동하거나 새로고침하거나 브라우저를 끌 때도 작동한다. 실행 시간을 오래 잡아먹는 스크립트라면 결과가 다를 수 있다. 아직 잘 몲.
비슷한 `onclose`가 있지만 지원하지 않는 브라우저가 있다.

### Window: beforeunload event

[https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event)  
[https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload](https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload)


문서가 언로드 되기 직전에 발생하는 이벤트. 이 이벤트를 사용하면 사용자가 페이지를 떠날 때 확인 대화 상자를 띄울 수 있다:

```js
window.addEventListener('beforeunload', (event) => {
  event.preventDefault();
  event.returnValue = '';
});
```

일부 브라우저에서 지원하지 않는 기능이 있으니 MDN 문서를 확인할 것.

### Window: hashchange event

[https://developer.mozilla.org/en-US/docs/Web/API/Window/hashchange_event](https://developer.mozilla.org/en-US/docs/Web/API/Window/hashchange_event)

URL의 해시가 변경됐을 때 발생하는 이벤트

```js
window.addEventListner('hashchange', callbackFn);
```

### Element: click event

[https://developer.mozilla.org/en-US/docs/Web/API/Element/click_event](https://developer.mozilla.org/en-US/docs/Web/API/Element/click_event)

요소가 클릭됐을 때 발생하는 이벤트. `<input type="checkbox">`에 click 이벤트 핸들러 부착 후 `event.preventDefault()`를 호출하면 체크/체크해제를 막을 수 있다. 비활성(disabled) 상태인 요소의 클릭에는 반응하지 않는다.

### HTMLElement: change event

[https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event)

`<input>`, `<select>`, `<textarea>`에서 값이 변화한 후에 발생하는 이벤트. `<input>`은 사용자가 전과 다른 값을 입력하고 포커스를 잃을 때 발생한다.

TODO

### HTMLElement: input event

[https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/input_event](https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/input_event)

이벤트 발생 시점은 keyup과 비슷함.

TODO

### HTMLElement: beforeinput event

[https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/beforeinput_event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/beforeinput_event)

`<input>` 혹은 `<textarea>` 태그에서 값이 수정되려 할 때 발생하는 이벤트. 체크박스의 체크/체크헤제에는 반응하지 않는다.

```js
document.querySelector('#text-beforeinput').addEventListener('beforeinput', event => console.log(event.target.value));
```

`event.target.value`는 수정 전의 값을 갖고 있어서 아무것도 입력을 하지 않은 최초에는 `<empty string>`이 출력된다.
