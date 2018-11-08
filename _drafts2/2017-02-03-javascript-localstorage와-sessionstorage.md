---
layout: post
date: 2017-02-03 13:42:00 +0900
title: 'JavaScript: localStorage와 sessionStorage'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - localstorage
  - sessionstorage
---

* Kramdown table of contents
{:toc .toc}

localStorage

sessionStorage

sessionStorage는 WAS에서 말하는 세션과 다르게 Window 객체가 기준이다. 새 창이나 새 탭을 열면 이전 창과 Window가 다르므로 sessionStorage를 공유하지 않는다.

쿠키와 뭐가 다른가?

**주의**: webstorage는 데이터를 도메인 기준으로 관리하는데 이때 scheme(http와 https)을 구분한다. 무슨 말이냐 하면 일반 프로토콜과 SSL 프로토콜간에 같은 이름이어도 다른 데이터로 인식하며 서로 접근할 수 없다.
