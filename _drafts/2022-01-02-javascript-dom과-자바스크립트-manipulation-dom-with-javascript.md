---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Manipulation'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - manipulation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)

## 개요

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 조작 관련 메서드 정리.

## append

### Node.appendChild()

```html
<button onclick="appendIframe()">눌러</button>
<div id="target"></div>

<script>
function appendIframe() {
  var iframe = document.createElement('iframe');
  iframe.src = 'http://noritersand.tistory.com';
  document.querySelector('#target').appendChild(iframe);
}
</script>
```

## prepend

## before

## after
