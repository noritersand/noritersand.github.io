---
layout: post
date: 2014-04-10 09:19:00 +0900
title: '[Web API] 새 창 혹은 팝업 창에서 submit'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - html-standard
  - submit
  - open
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


```js
var f = document.form(0);

window.open('', 'WINDOW_NAME', 'toolbar=no, location=no, status=no,'
    + 'menubar=no, scrollbars=no, resizable=no, width=500, height=270');

f.action = '/test/something.action';
f.method = 'post';
f.target = 'WINDOW_NAME';
f.submit();
```

`window.open()`을 사용하되 URL은 없고 이름만 있는 (내용이 비어있는)창을 열어 그 창을 타깃으로 submission 하는 방식이다. 새 창에서 요청을 생성하되 HTTP 포스트 메서드가 필요한 경우 사용한다.

익스에서 창이 두 개가 뜨거나 부모창이 닫히는 현상이 있어서 위 방법보단, `window.open()`으로 빈 페이지를 불러오지 말고 submission 스크립트를 갖고 있는 페이지를 로딩한 뒤 로딩된 페이지가 자기 자신을 submission 하도록 하는 방식이 낫다.
