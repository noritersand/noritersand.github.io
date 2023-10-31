---
layout: post
date: 2017-02-03 13:42:10 +0900
title: '[Web API] iframe contents'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - dom-standard
  - html-standard
  - iframe
---

* Kramdown table of contents
{:toc .toc}


자바스크립트로 iframe 객체에 접근하는 방법은 두 가지가 있는데, 하나는 document 객체의 메서드를 이용하는 방법이고 두 번째는 window 객체의 frames 프로퍼티로 접근하는 방법이다.

```js
// 1 - use get method
var winObj = document.getElementById('FRAME_ID').contentWindow;
var docdObj = document.getElementById('FRAME_ID').contentDocument;

// 2 - property
var FRAME_NAME = window.frames[0].name; //return 'myFrame'
var firstFrm = window.frames.FRAME_NAME;
var secondFrm = window.frames['myFrame'];
var winObj2 = firstFrm.window; // 혹은 firstFrm;
var docObj2 = secondFrm.document;

winObj == winObj2; // true
docObj == docObj2; // true
```

단, 여기서 사용한 contentDocument, contentWindow는 모질라 기반에서만 지원하고 사파리, 오페라에서는 사용할 수 없다.

jQuery에 `.contents()`라는 메서드가 있는데 가져오는 내용이 다르다. (node 전체를 긁어옴)
