---
layout: post
date: 2014-02-17 14:25:00 +0900
title: 'jQuery: 비동기 파일 전송'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - formdata
  - ajax
  - async
  - file
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/ko/docs/Web/API/FormData](https://developer.mozilla.org/ko/docs/Web/API/FormData)

#### 브라우저 호환

- IE9와 그 이하에서는 FormData 사용불가

쫑알쫑알.

```html
<body>
    <form id="submitForm" enctype="multipart/form-data">
        <input name="attachFile" id="attachFile" type="file">
        <button type="button" id="submitBtn">upload</button>
    </form>
</body>
```

```html
<script>
  $(document).ready(function() {
    $('#submitBtn').click(function() {
      var data = new FormData();
      $.each($('#attachFile')[0].files, function(i, file) {
        data.append('file-' + i, file);
      });

      $.ajax({
        url: '/upload.action',
        type: "post",
        dataType: "text",
        data: data,
        // cache: false,
        processData: false,
        contentType: false,
        success: function(data, textStatus, jqXHR) {
          alert(data);
        }, error: function(jqXHR, textStatus, errorThrown) {}
      });
    });
  });
</script>
```

첨부된 파일을 multipart/form-data 형식으로 처리하기 위해 FormData 객체를 이용했다. FormData는 객체는 append(key, value) 메서드 하나만 있는 객체로 XMLHttpRequest를 통해 파일을 전송할 수 있게 한다.

### response example

```java
@RequestMapping(value = "/upload", method = RequestMethod.POST)
@ResponseBody
public String upload(MultipartHttpServletRequest req,
  HttpServletResponse res) {

  Iterator<String> itr = req.getFileNames();
  MultipartFile mpf = req.getFile(itr.next());
  String originFileName = mpf.getOriginalFilename();

  // ... 생략

  return "success";
}
```
