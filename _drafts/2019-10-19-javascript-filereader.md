---
layout: post
date: 2019-10-19 23:11:00 +0900
title: '[JavaScript] FileReader'
categories:
  - javascript
tags:
  - javascript
  - file-api
  - filereader
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)
- [\[MDN\] File](https://developer.mozilla.org/en-US/docs/Web/API/File)


## 개요

TODO 설명

파일 읽기를 지원하는 웹 API


## 기본 사용법

```html
<input type="file" onchange="handleFileChange(event)">
<script>
var actualResponse;

function handleFileChange(e) {
  console.log('files:' , e.target.files);

  let file = e.target.files[0];
  if (!file) {
    return;
  }

  var reader = new FileReader();
  reader.addEventListener('loadend', e => {
    actualResponse = e.target.result;
    console.log('actualResponse:', actualResponse);
  });
  reader.readAsDataURL(file);
}
</script>
```

여기서 파일을 첨부하면 `actualResponse`에 `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABiIAAAQ8CAYAAAAfYY...` 이런 모양의 값이 할당된다. 이 값은 data URI scheme이라고 하는데, 텍스트나 이미지 등의 데이터를 URL 형태로 나타낸 것이다.

### FileReader로 읽은 값을 이미지로 활용하기

만약 첨부한 파일이 이미지인 경우 이 data URI scheme 값을 그대로 `<img>` 태그 `src` 속성에 할당하면 마치 외부 이미지 파일을 가져온 것과 같은 결과가 나타난다.

```html
<img src alt="이미지 태그">
<button type="button" onclick="setImageSrc()">첨부 파일을 이미지로</button>
<script>
function setImageSrc() {
  document.querySelector('img').src = actualResponse;
}
</script>
```

### FormData로 전송하기

덧붙여서, FormData로 서버에 전송하려면 다음처럼 작성한다:

```html
<button type="button" onclick="sendFormData()">FormData로 전송</button>
<script>
function sendFormData() {
  let formData = new FormData();
  formData.append('file', actualResponse);
  console.log('formData:', formData);

  fetch('/get-my-request-body.data', {
    method: 'POST',
    body: formData
  })
  .then(response => response.text())
  .then(data => console.log(data))
  .catch(error => console.error(error));
}
</script>
```

이 때 요청 헤더의 `Content-Type`은 `multipart/form-data`다.


## 브라우저가 FileReader를 지원하는지 확인하기

```js
Boolean(window.File && window.FileReader && window.FileList && window.Blob)
```
