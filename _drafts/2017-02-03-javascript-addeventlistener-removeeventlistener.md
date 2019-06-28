---
layout: post
date: 2017-02-03 13:47:00 +0900
title: 'JavaScript: addEventListener, removeEventListener'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - eventlistener
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)
- [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)

## 이벤트 버블링 event bubbling

## 이벤트 캡처링 event capturing

## EventTarget.addEventListener()

```
EventTarget.addEventListener( type, listener [ , useCapture ] );
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

```
sdf
```

- `aa`:

sdf
