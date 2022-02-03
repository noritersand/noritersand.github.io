---
layout: post
date: 2017-02-03 13:42:00 +0900
title: '[JavaScript] localStorage와 sessionStorage'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - html-standard
  - localstorage
  - sessionstorage
---

* Kramdown table of contents
{:toc .toc}

## localStorage

localStorage는 브라우저 전체에서 공유되는 저장공간이다. 여러 창(혹은 탭)끼리도 같은 저장공간을 사용한다.

## sessionStorage

sessionStorage는 localStorage와 다르게 Window마다 분리되는 저장공간이다. 브라우저의 다른 창(혹은 탭)에서는 Window가 다르므로 sessionStorage를 공유하지 않는다. **Java Servlet에서 말하는 세션과는 다른 개념**이므로 혼동하지 말자. 브라우저를 종료하면 자동으로 초기화된다.

### 쿠키와 뭐가 다른가?

**주의**: 브라우저는 localStorage와 sessionStorage를 호스트 기준으로 관리하는데, 이때 호스트에는 scheme(http와 https)이 포함된다. 무슨 말이냐 하면 일반 프로토콜과 SSL 프로토콜간에 같은 이름이어도 다른 데이터로 인식하며 서로 접근할 수 없다.
