---
layout: post
date: 2017-06-09 15:29:00 +0900
title: '[misc] CKEditor가 소스의 속성(클래스 등)을 제거하는 현상이 있을 때'
categories:
  - misc
tags:
  - ckeditor
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](/somewhere)

```js
var polcDescEditor = CKEDITOR.replace('polcDesc', {
  extraAllowedContent: '*(*){*}[*]'
} );
```
