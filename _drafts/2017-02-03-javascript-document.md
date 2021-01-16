---
layout: post
date: 2017-02-03 13:46:00 +0900
title: '[JavaScript] Document'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - document
  - html
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/API/Document](https://developer.mozilla.org/ko/docs/Web/API/Document)

## 주요 프로퍼티

### forms

## 주요 메서드

### document.createElement()

### document.body.appendChild()

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
