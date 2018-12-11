---
layout: post
date: 2017-04-10 16:53:00 +0900
title: 'JavaScript: 설정된 모든 timeout 지우기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - timeout
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```js
var highestTimeoutId = setTimeout(";");
for (var i = 0 ; i < highestTimeoutId ; i++) {
  clearTimeout(i);
}
```
