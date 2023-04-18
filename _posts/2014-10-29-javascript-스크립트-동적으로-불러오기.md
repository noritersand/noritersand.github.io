---
layout: post
date: 2014-10-29 13:32:00 +0900
title: '[JavaScript] 스크립트 동적으로 불러오기'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - html-standard
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### pure JavaScript

head 태그 안쪽에 DOM 생성:

```js
var headTag = document.getElementsByTagName("head")[0];
var newScript = document.createElement('script');
newScript.type = 'text/javascript';
newScript.onload = function() { console.log('자바스크립트 로드 완료') };
newScript.src = 'somescript.js';
headTag.appendChild(newScript);
```

#### jQuery

`jQuery.getScript()` 사용:

```js
$.getScript("somescript.js", function ( data, textStatus, jqxhr ) {
  console.log(data); // 받은 data
  console.log(textStatus); // success
  console.log(jqxhr.status); // 200
});
```
