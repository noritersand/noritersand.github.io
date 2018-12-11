---
layout: post
date: 2017-01-06 16:16:00 +0900
title: 'jQuery: 파일 첨부 이벤트에 submit 하기'
categories:
  - javascript
  - jquery
tags:
  - javascript
  - jquery
  - file upload
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

#### required

- jQuery form plugin: [http://malsup.com/jquery/form/#download](http://malsup.com/jquery/form/#download)

```html
<!-- DB 저장 폼 -->
<form>
    <input type="text" id="filePath" name="filePath" />
    <button type="button" onclick="upload()">파일첨부</button>
</form>

<!-- 파일 업로드 폼 -->
<form id="fileForm" style="display: none;">
    <input type="file" id="fileInput" name="fileInput">
</form>

<script>
  function upload() {
    var $oldFileInput = $('#fileInput');
    var $newFileInput = $('#fileInput').clone();
    $oldFileInput.one('change', function() {
      $('#fileForm').ajaxSubmit({
        method: 'post',
        url: '/uploadFile.do',
        success: function(data) {
          // data: 서버가 반환해야할 파일의 웹 경로를 의미한다.
          $('#filePath').val(data);
        }, complete: function(data) {
          $oldFileInput.replaceWith($newFileInput);
          $oldFileInput.remove();
        }
      });
    });
    $oldFileInput.click();
  }
</script>
```
