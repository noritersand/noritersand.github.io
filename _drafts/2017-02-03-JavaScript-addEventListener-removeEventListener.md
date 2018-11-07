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

#### 관련 문서

- [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
- [https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)

## EventTarget.addEventListener()

```
EventTarget.addEventListener( type, listener [ , useCapture ] );
```

- `type`:
- `listener`:
- `useCapture`:

```js
<button type="button" id="btn">push me</button>
<script>
  var foo = document.querySelector('#btn');
  foo.addEventListener('click', function() {
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
