---
layout: post
date: 2015-06-30 11:00:00 +0900
title: 'Servlet: referrer? referer?'
categories:
  - java
  - servlet
tags:
  - java
  - servlet
  - referrer
---

HTTP 명세에는 referrer의 r이 하나 빠져있다. 우습게도 오타 때문이라 하는데 이 때문인지 서블릿에서는 헤더의 referrer를 가져올 때 r을 하나 뺀 'referer'로 명시해야한다. 그에 비해 자바스크립트에선 철자가 틀리지 않은 'referrer'를 사용한다.

```js
document.referrer; // "http://noritersand.tistory.com/410"
```
