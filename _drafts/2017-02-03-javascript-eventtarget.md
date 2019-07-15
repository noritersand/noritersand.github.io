---
layout: post
date: 2017-02-03 13:47:00 +0900
title: 'JavaScript: EventTarget'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - eventlistener
  - eventtarget
  - dispatch
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)
- [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)

## 이벤트 버블링 event bubbling

## 이벤트 캡처링 event capturing

## EventTarget.addEventListener()

```
EventTarget.addEventListener( type, listener [ , useCapture ] )
```

- `type`: 이벤트 종류
- `listener`: 이벤트가 발생하면 실행할 함수. 유일한 인자로 event 객체가 전달된다.
- `useCapture`: 캡처링 사용 여부. 생략하면 false

```js
<button type="button" id="btn">push me</button>
<script>
  var foo = document.querySelector('#btn');
  foo.addEventListener('click', function(event) {
    alert('who? me?');
  });
</script>
```

## EventTarget.removeEventListener()

```js
sdf
```

- `aa`:

sdf

## EventTarget.dispatchEvent()

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
