---
layout: post
date: 2017-02-03 13:46:00 +0900
title: '[JavaScript] Document'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - html-standard
  - html
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)

## 주요 프로퍼티

### forms

## 주요 메서드

### [document.createElement()](https://developer.mozilla.org/ko/docs/Web/API/Document/createElement)

### [Node.appendChild()](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)

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

### [Document.createTextNode()](https://developer.mozilla.org/ko/docs/Web/API/Document/createTextNode)

```js
// 스크립트로 인터널 CSS 추가하기
var headTag = document.querySelector('head');
var styleTag = document.createElement('style');
headTag.appendChild(styleTag);
styleTag.setAttribute('type', 'text/css');

var css = 'h1 { background: red; }';
styleTag.appendChild(document.createTextNode(css));
```
