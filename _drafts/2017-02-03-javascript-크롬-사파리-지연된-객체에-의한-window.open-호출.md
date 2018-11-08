---
layout: post
date: 2017-02-03 13:42:00 +0900
title: 'JavaScript: 크롬/사파리 - 지연된 객체에 의한 window.open() 호출'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - window.open
---

* Kramdown table of contents
{:toc .toc}

크롬과 사파리에선 setTimeout() 의 콜백에서 window.open()이 불가능하다. 꼼수로 setTimeout() 실행 전 새 창을 미리 열고 콜백에선 미리 열어둔 창의 주소만 바꾸는 방법이 있으나... 창이 미리 열려서 꼴뵈기 싫은 단점이 있다.
