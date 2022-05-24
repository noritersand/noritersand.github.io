---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Creation'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - creation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)

## 개요

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 생성 관련 메서드 정리.

## Document.prototype.createElement()

```js
var $body = document.querySelector('body');
var $p = document.createElement('p');
$p.innerHTML = 'Hello world!';
$body.appendChild($p);
```

## Document.prototype.createTextNode()

```js
// 스크립트로 인터널 CSS 추가하기
var $head = document.querySelector('head');
var $style = document.createElement('style');
$head.appendChild($style);
$style.setAttribute('type', 'text/css');

var css = 'h1 { background: red; }';
$style.appendChild(document.createTextNode(css));
```
