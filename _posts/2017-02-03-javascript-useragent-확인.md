---
layout: post
date: 2017-02-03 13:42:10 +0900
title: '[JavaScript] userAgent 확인'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - html-standard
  - useragent
---

```html
<script>
  function chkUsrAgent() {
    prompt('Copy to clipboard: Ctrl+C, Enter', window.navigator.userAgent);
  }
</script>
<button type="button" onclick="chkUsrAgent()">what's my user agent</button>
```

<script>
  function chkUsrAgent() {
    prompt('Copy to clipboard: Ctrl+C, Enter', window.navigator.userAgent);
  }
</script>
<button type="button" onclick="chkUsrAgent()">what's my user agent</button>
