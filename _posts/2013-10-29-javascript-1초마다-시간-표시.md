---
layout: post
date: 2013-10-29 19:00:00 +0900
title: '[JavaScript] 1초마다 시간 표시'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - time
  - interval
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```html
<script>
  window.onload = function() {
    setInterval(function() {
      var target = document.getElementsByName("dt_now")[0];
      target.value = new Date();
    }, 1000);
  }
</script>

<input type="text" size="32" name="dt_now" value="00" />
```
