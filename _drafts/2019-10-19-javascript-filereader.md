---
layout: post
date: 2019-10-19 23:11:00 +0900
title: '[JavaScript] FileReader'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - filereader
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)
- [\[MDN\] File](https://developer.mozilla.org/en-US/docs/Web/API/File)

웹 앱의 파일 읽기를 지원하는 API.


#### 브라우저가 FileReader를 지원하는지 확인하는 스크립트

```js
Boolean(window.File && window.FileReader && window.FileList && window.Blob)
```
