---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[Web API] DOM 다루기: Event Handling'
categories:
  - web-api
tags:
  - web-api
  - javascript
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

#### 참고 문서

- [Document \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [EventTarget \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)
- [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)


## 개요

내 맴대로 분류한 DOM 이벤트 핸들링 관련 API 정리.


## 이벤트의 버블링과 캡처링 Event Bubbling, Event Capturing

버블링은 이벤트가 발생한 엘리먼트에서 루트로 올라가는 과정, 캡처링은 반대로 이벤트가 루트에서 이벤트가 발생한 엘리먼트까지 내려가는 과정을 말한다.

이벤트가 발생하면 캡처링과 버블링 단계가 차례대로 수행된다. 실행 순서는 다음과 같다:

1. 캡처링 단계: 최상위 부모 엘리먼트에서 타깃 엘리먼트로 이벤트가 전파됨
2. 타깃에서 처리: 이벤트가 타깃 엘리먼트에서 처리됨
3. 버블링 단계: 타깃 엘리먼트에서 다시 부모 엘리먼트로 이벤트가 전파됨

```js
const parent = document.querySelector('#parent');
const child = document.querySelector('#child');

parent.addEventListener('click', () => {
  console.log('부모 - 캡처링');
}, true);  // 캡처링

parent.addEventListener('click', () => {
  console.log('부모 - 버블링');
}); // 버블링

child.addEventListener('click', () => {
  console.log('자식 - 일반');
});

// child 클릭시 로그 순서:
// 1. "부모 - 캡처링"    <- useCapture: true라서 가장 먼저 실행
// 2. "자식 - 일반"      <- 실제 클릭된 타깃의 리스너
// 3. "부모 - 버블링"    <- 버블링 단계에서 실행
```


## 인스턴스 메서드

### EventTarget.prototype.addEventListener()

지정한 유형의 이벤트를 대상이 수신할 때마다 호출할 함수를 설정하는 메서드. 흔히 엘리먼트에 이벤트 콜백 함수를 부착한다고 한다.

```
addEventListener(type, listener)
addEventListener(type, listener, options)
addEventListener(type, listener, useCapture)
```

- `type`: 이벤트 종류
- `listener`: 이벤트가 발생하면 실행할 함수. 유일한 인자로 Event 객체가 전달된다.
- `useCapture`: 리스너가 캡처링 단계에서 실행될지를 결정하는 값. `true`로 설정하면 실제 이벤트가 발생한 엘리먼트의 리스너보다 상위 엘리먼트의 리스너가 먼저 실행된다. 생략했을 때의 기본값은 `false`로, 버블링 단계에서 리스너가 실행된다.
- `options`:
  - `capture`: 기본값은 `false`. `true`로 지정하면 이벤트가 하위 요소로 전파되는 캡처링 단계에서 이 리스너가 호출된다. DOM 트리의 최상위(window)에서부터 이벤트 타깃을 향해 내려가는 도중에 이벤트를 가로채서 먼저 처리할 때 사용한다.
  - `once`: 기본값은 `false`. `true`인 경우 리스너가 발동된 직후 제거된다.
  - `passive`: 리스너 내부에서 `event.preventDefault()`를 절대로 호출하지 않겠다고 브라우저에 약속하는 옵션. `touchmove`나 `wheel` 같은 스크롤 관련 이벤트가 발생할 때, 브라우저는 리스너 내부에서 `preventDefault()`가 호출되어 스크롤을 막을지 아닐지 알 수 없다. 그래서 리스너 실행이 끝날 때까지 스크롤 처리를 멈추고 대기하므로 스크롤 프레임 드랍(Jank)이 발생한다. `passive: true`를 주면 브라우저가 리스너 실행을 기다리지 않고 즉시 스크롤을 진행하므로 성능이 대폭 향상된다. `touchstart`, `touchmove` 및 최신 브라우저의 `wheel`, `mousewheel` 이벤트는 성능 향상을 위해 기본값이 `true`로 설정되어 있다. 만약 이 이벤트들에서 `preventDefault()`를 쓰려면 명시적으로 `{ passive: false }`를 전달해야 한다.
  - `signal`: `AbortSignal` 객체를 전달하여 이벤트 리스너를 원격으로 제어/제거할 수 있게 해주는 옵션. 여러 리스너를 한 번에 제거해야 하거나, 컴포넌트가 언마운트될 때 관련 리스너를 깔끔하게 일괄 정리할 때 매우 유용하다.

```js
function clickHandler(event) {
  alert('who? me?');
}
element.addEventListener('click', clickHandler);
```

```js
// 스크롤 성능을 극대화하는 패시브 리스너
window.addEventListener('touchmove', onTouchMove, { passive: true });
```

```js
const controller = new AbortController();

// controller.signal을 전달
window.addEventListener('resize', handleResize, { signal: controller.signal });
window.addEventListener('scroll', handleScroll, { signal: controller.signal });

// 원할 때 단 한 줄로 등록된 모든 리스너 일괄 제거
controller.abort();
```

### EventTarget.prototype.removeEventListener()

이벤트 대상에 등록한 수신기(=함수)를 제거하는 메서드.

```
removeEventListener(type, listener)
removeEventListener(type, listener, options)
removeEventListener(type, listener, useCapture)
```

- `type`: 제거할 이벤트의 종류 (예: `'click'`)
- `listener`: 메모리에서 제거할 정확한 핸들러 함수 참조. 익명 함수로 등록한 경우 일치하는 참조를 찾을 수 없어 제거가 불가능하다.
- `useCapture`: 제거하려는 이벤트 핸들러가 캡처링 단계에 등록된 것인지 여부를 나타내는 불리언 값. `true`면 캡처링 단계의 리스너를, `false`(기본값)면 버블링 단계의 리스너를 제거한다. 등록 시 `useCapture`를 `true`로 설정했다면 제거할 때도 반드시 `true`를 전달해야 정상적으로 제거된다.
- `options`:
  - `capture`: `useCapture`와 동일한 역할을 하는 옵션. 등록 시 `addEventListener(type, listener, { capture: true })` 형태로 등록했다면, 제거 시에도 `{ capture: true }`를 전달해야 제거된다.

```js
element.removeEventListener('click', clickHandler);
```

### EventTarget.prototype.dispatchEvent()

DOM 이벤트를 수동으로 발동한다. 이런 식으로  발생하는 이벤트를 '인공 이벤트(synthetic events)'라고 한댄다.

```
target.dispatchEvent(event)
```

- `target`: 이벤트를 발생시킬 엘리먼트
- `event`: 디스패치될 event 객체

```js
var event = new Event('build');

// 이벤트 수신
element.addEventListener('build', function (e) { /* ... */ }, false);

// 이벤트 발동
element.dispatchEvent(event);
```

ℹ️ 초로미 개발자 도구를 이용해서 디버깅할 때, `dispatchEvent()`를 활용해서 마우스오버 이벤트를 강제로 발생시키는 꼼수가 있다:

```js
// 초로미 개발자 도구의 Elements 패널에서 클릭한 요소는 $0 변수에 저장된다.
$0.dispatchEvent(new MouseEvent('mouseover', { bubbles: true }));
```

### HTMLElement.prototype.click()

클릭 이벤트를 강제로 발생시키는 메서드.

```js
document.querySelector('#input').click();
```


## 이벤트 목록

### Window: load event

문서 내의 모든 HTML, CSS, 이미지, 폰트, 프레임 등의 외부 리소스가 모두 완전히 로드되었을 때 발생하는 이벤트.

```js
window.addEventListener('load', (event) => {
  console.log('페이지 내의 모든 리소스(이미지 포함) 로딩 완료');
});
```

### DOMContentLoaded

HTML 파싱이 완료되어 DOM 트리가 완전히 구축되었을 때 발생하는 이벤트. 이미지까지 다 불러왔는지는 이 이벤트로 알 수 없다.

```js
document.addEventListener('DOMContentLoaded', (event) => console.log('DOM fully loaded'));
```

ℹ️ jQuery의 `$(document).ready()`로 잘 알려진 그 이벤트임

#### 왜 MDN의 DOMContentLoaded 문서는 두 개일까? 🤔

- <https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event>
- <https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event>

이벤트의 실제 발생 타깃은 **Document**다. 하지만 DOM 이벤트의 버블링 특성에 의해 Document에서 발생한 이벤트가 상위인 Window까지 전파된다. 

따라서 `document.addEventListener('DOMContentLoaded', ...)`로 수신하는 것이 규격에 맞는 정석이지만, 버블링 덕분에 `window.addEventListener(...)`로도 동일하게 수신할 수 있어 MDN에 두 문서가 모두 존재한다.

### Window: pageshow event

페이지가 로드되거나, 뒤로가기/앞으로가기(bfcache - Back/Forward Cache)를 통해 복원될 때 발생하는 이벤트.

`load` 이벤트는 BFCache에서 페이지를 복원할 때는 다시 실행되지 않는 단점이 있는데, `pageshow` 이벤트의 `event.persisted` 속성을 활용하면 해당 페이지가 BFCache에서 복원된 것인지(`true`) 구분할 수 있다.

```js
window.addEventListener('pageshow', (event) => {
  if (event.persisted) {
    console.log('BFCache로부터 복원된 페이지입니다.');
  }
});
```

### Window: unload event

**🚫 사용금지된 이벤트**

~~문서가 언로딩(대충 다른 페이지로 이동 중일 때 쯤) 중일 때 발생하는 이벤트.~~

~~얼럿은 차단되지만 스크립트가 실행되긴 한다. 페이지를 이동하거나 새로고침하거나 브라우저를 끌 때도 작동한다. 실행 시간을 오래 잡아먹는 스크립트라면 결과가 다를 수 있다. 아직 잘 몲.~~

~~비슷한 `onclose`가 있지만 지원하지 않는 브라우저가 있다.~~

### Window: beforeunload event

- <https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event>
- <https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeunload>

문서가 언로드 되기 직전에 발생하는 이벤트. 이 이벤트를 사용하면 사용자가 페이지를 떠날 때 확인 대화 상자를 띄울 수 있다:

```js
window.addEventListener('beforeunload', (event) => {
  event.preventDefault();
  event.returnValue = '';
});
```

이 이벤트는 브라우저마다 지원하는 상세 기능과 작동 방식이 약간씩 다르다. MDN 문서를 확인할 것.

창을 닫는 것을 가로막으려면 화면에 존재하는 입력란에 사용자 액션이 발생해야 한다. 예를 들어 `display: none`으로 숨겨진 `<input>`을 마련한 뒤, 아무 사용자 액션이 발생했을 때 `<input>`의 `value`를 변경시킨 다음부턴 창을 닫을 때 컨펌창이 나타난다. 하지만 컨펌창의 메시지까지 제어할 수는 없는 것으로 보인다.

### Window: hashchange event

<https://developer.mozilla.org/en-US/docs/Web/API/Window/hashchange_event>

URL의 해시가 변경됐을 때 발생하는 이벤트

```js
window.addEventListner('hashchange', callbackFn);
```

### Window: popstate event

사용자가 브라우저의 뒤로가기/앞으로가기 버튼을 누르거나, JavaScript로 `history.back()`, `history.forward()`, `history.go()`를 호출하여 현재 히스토리 엔트리가 변경될 때 발생한다. `history.pushState()`나 `history.replaceState()` 호출 자체만으로는 `popstate` 이벤트가 발생하지 않는다.

ℹ️ SPA(Single Page Application)에서 라우팅 이동 및 뒤로가기 제어를 구현할 때 핵심이 되는 이벤트다.

```js
window.addEventListener('popstate', (event) => {
  console.log('현재 state:', event.state);
});
```

### Element: click event

엘리먼트가 클릭됐을 때 발생하는 이벤트. `<input type="checkbox">`에 click 이벤트 핸들러 부착 후 `event.preventDefault()`를 호출하면 체크/체크해제를 막을 수 있다.

⚠️ 비활성(disabled) 상태인 엘리먼트는 클릭 이벤트가 발생하지 않는다.

### Element: keydown event

키보드의 아무 키나 누를 때마다 발생하는 이벤트. 키보드 관련 이벤트 중 가장 먼저 발생한다.

`keydown` 이벤트 핸들러에서 `event.preventDefault()`를 호출하면 키 입력 자체(텍스트 상자에 문자가 찍히는 동작 등)를 취소시킬 수 있다.

키를 누르고 있으면(Hold) 이벤트가 계속 반복해서 발생한다. 이 때 `event.repeat` 속성이 `true`가 된다.

한글 등 IME 입력 시 2번 실행되는 현상이 발생하므로, `event.isComposing` 확인 로직이 필요하다. (조 아래에 정리함)

#### KeyboardEvent

입력 이벤트와 엮여서 아주 자주 사용하게 될 이벤트 객체. 사용자가 키보드 장치의 키를 눌렀을 때, 어떤 키가 눌렸는지, 조합 키(<kbd>ctrl</kbd>, <kbd>alt</kbd>, <kbd>shift</kbd>, ...)가 사용되었는지 등을 설명한다. `keydown`, `keyup` 이벤트에서만 생성된다.

**인스턴스 프로퍼티**:

- `key`: 사용자가 입력한 문자의 값 (예: `"a"`, `"Enter"`, `"ArrowLeft"`).
- `code`: 누른 키의 물리적 위치 (예: `"KeyA"`, `"Digit1"`).
- `altKey`, `ctrlKey`, `shiftKey`, `metaKey`: 조합 키가 눌렸는지 여부를 나타내는 불리언 값.
- `repeat`: 키를 계속 누르고 있어 이벤트가 반복적으로 발생하는지 여부.
- `isComposing`: 한글 입력처럼 IME를 거치는 문자의 완성 과정(조합) 중인지를 나타내는 속성

`isComposing`은 조합중인 한글 입력이 문제가 되는 현상을 해소할 때 사용하는 속성이다. 한글을 입력할 때는 `keydown` 이벤트에서 `true`로 넘어온다.

```js
inputElement.addEventListener('keydown', (e) => {
  if (e.isComposing) {
    return; // 조합 중일 때는 보정 로직을 실행하지 않고 종료
  }
});
```

ℹ️ `isComposing`은 `InputEvent`에서도 제공된다.

### Element: keypress event

🚫 keypress 이벤트는 deprecated 되었으므로 사용하지 말 것.

### HTMLElement: beforeinput event

`<input>` 혹은 `<textarea>` 태그에서 값이 수정되려 할 때 발생하는 이벤트. 사용자가 입력을 시도할 때마다 이벤트가 발생하므로, 실시간으로 입력을 검증하거나 특정 입력을 제한할 때 유용하다. 

```js
inputElement.addEventListener('beforeinput', function(event) {
  console.log('value before changed:', event.target.value);
  // 특정 조건에서 입력 막기
  if (/* 어떤 조건 */) {
    event.preventDefault(); // 입력 값 변경 방지
  }
});
```

`event.target.value`에는 변경되기 전의 값이 들어있다. 이 이벤트는 `input`이나 `change`보다 먼저 발생하기 때문에 이벤트 핸들러에서 `event.preventDefault()`를 호출하면 값의 변경을 막을 수 있다. 아무것도 입력을 하지 않은 시점의 `event.target.value` 값은 `<empty string>`이다.

⚠️ `<select>` 태그의 항목 선택과 `<input type="checkbox">`나 `<input type="radio">`의 체크/체크헤제에는 반응하지 않는다.

ℹ️ 체크박스와 라디오의 값 변경을 막으려면 `click` 이벤트 핸들러에서 `event.preventDefault()`를 호출하면 됨

### HTMLElement: input event

`<input>`, `<textarea>`, `<select>`에서 사용자가 무언가를 입력할 때마다 발생하는 이벤트. `keydown`, `beforeinput` 이벤트보다 늦게 발생한다.

이벤트 핸들러가 등록된 엘리먼트는 `event.currentTarget` 프로퍼티가, 실제 이벤트가 발생한 엘리먼트는 `event.target` 프로퍼티가 가리킨다.

전화번호 자동 포맷처럼 사용자 입력을 실시간으로 필터링하고 변경하는 기능을 구현할 때는 `input` 이벤트가 가장 적절하다. 키보드 입력뿐만 아니라 복사/붙여넣기 등 모든 입력 방식을 커버하기 때문.

`key`나 `keyCode`가 제공되지 않는다. 키 입력을 제어하려면 `keyup`이나 `keydown`을 쓰자.

ℹ️ TypeScript 환경에서 `event.target`은 `EventTarget` 타입으로 받아온다. `EventTarget`은 `value`가 없으므로 필요한 경우 `HTMLInputElement` 같은 서브 타입으로 '타입 단언'을 해야 한다.

```ts
const handler = (e: Event) => {
  const value = (e.target as HTMLInputElement).value;
};
```

### HTMLElement: change event

`<input>`, `<textarea>`, `<select>`에서 값이 변화한 후에 발생하는 이벤트. `<input>` 태그의 타입이 `text`, `email`, `password` 같은 텍스트 계열인 경우, **값이 변경되었고** 포커스를 잃을 때 발생한다. 이 외에 체크박스, 라디오, 셀렉트박스, 파일은 값을 바꾸거나 파일을 첨부했을 때 즉시 발생한다.

`input` 이벤트보다 발생 시점이 늦다.

ℹ️ React에서는 작동 방식이 약간 달라지는데, 텍스트 계열 `<input>`에서 **포커스와 관계 없이 값이 변경될 때마다** change 이벤트가 발생한다.
