---
layout: post
date: 2017-02-03 13:38:10 +0900
title: '[JavaScript] 첨부파일에 접근하기'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - attachment
  - file
---

```html
<form id="submitForm">
    <input type="file" name="attachFile"/>
</form>
```

`<input type="file">`을 통해 파일이 첨부되었을때 첨부된 파일에 대한 정보는 해당 input객체의 files 프로퍼티가 갖고 있다.

```js
var attFile = document.forms[0].attachFile.files;
```

여기서 파일의 크기는 다음처럼 구한다:

```js
attFile.items[0].size;
```

단, IE9(혹은 IE10 호환성 모드)와 그 이하에서는 FILE API를 지원하지 않는다. 악의축마이크로소프트

그러니 호환 짱짱맨 jQuery.fileUpload 플러그인 쓰세여
