---
layout: post
date: 2013-11-29 00:00:00 +0900
title: '[jQuery] blueimp 파일 업로드 플러그인'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - event
  - handler
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://github.com/blueimp/jQuery-File-Upload](https://github.com/blueimp/jQuery-File-Upload)

#### required

- jQuery 1.6 or higher
- jQuery UI widget factory 1.9 or higher
- jQuery Iframe Transport plugin

#### 지원하는 브라우저

- [Browser support](https://github.com/blueimp/jQuery-File-Upload/wiki/Browser-support)

## request example

```html
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>sample</title>
<script src="http://code.jquery.com/jquery-1.9.1.js"></script>
<script src="http://code.jquery.com/ui/1.10.3/jquery-ui.js"></script>
<script src="../lib/jquery.fileupload.js"></script>
<script >
  $(document).ready(function() {
    $('#upFile').fileupload({
      url : '/bo/sample/file/uploadProcess.do',
      dataType: 'json',
      //replaceFileInput: false,
      add: function(e, data){
        var uploadFile = data.files[0];
        var isValid = true;
        if (!(/png|jpe?g|gif/i).test(uploadFile.name)) {
          alert('png, jpg, gif 만 가능합니다');
          isValid = false;
        } else if (uploadFile.size > 5000000) { // 5mb
          alert('파일 용량은 5메가를 초과할 수 없습니다.');
          isValid = false;
        }
        if (isValid) {
          data.submit();
        }
      }, progressall: function(e,data) {
        var progress = parseInt(data.loaded / data.total * 100, 10);
        $('#progress .bar').css(
          'width',
          progress + '%'
        );
      }, done: function (e, data) {
        var code = data.result.code;
        var msg = data.result.msg;
        if(code == '1') {
          alert(msg);
        } else {
          alert(code + ' : ' + msg);
        }
      }, fail: function(e, data){
        // data.errorThrown
        // data.textStatus;
        // data.jqXHR;
        alert('서버와 통신 중 문제가 발생했습니다');
        foo = data;
      }
    });
  });
</script>
</head>
<body>
<input type="file" name="fileData" id="upFile">
    <div id="progress">
        <div class="bar" style="width: 0%;"></div>
    </div>
</body>
</html>
```

파일이 선택될 경우 즉시 업로드하며  서버가 업로드 완료 후 서버에서 전달한 데이터를 표시한다.

#### Options

- `dataType`: 서버에서 응답받을 데이터의 타입. (xml, json, script, or html)
- `replaceFileInput`: 기본값은 true, 파일이 첨부되면 이벤트 핸들링 시점에서 파일입력폼을 클론으로 대체한다. 이 값이 false면 fileUpload 이벤트 후에도 파일입력폼의 첨부파일이 사라지지 않는다. 만약 fileUpload 플러그인을 업로드 용도가 아닌 파일의 유효성 검사를 체크하기 위해서 사용한다면 이 옵션을 false로 하여 사용한다.

life cycle은 add - progress - done or fail

add에서 submission하지 않으면 데이터 전송이 이뤄지지 않으니 주의할 것. 이를 이용하여 validation만 적용하는 꼼수가 있다.

## response example

```java
@RequestMapping(value="/sample/file/upload", produces="application/json")
@ResponseBody
public String upload(HttpServletRequest req) throws IOException {

    // ... 생략

    return "{\"code\":\"1\", \"msg\":\"file upload success.\"}";
}
```

## 파일첨부 시 즉시 검증

FileUpload plugin을 사용하면 파일첨부 시점에 add 메서드가 실행되는 점을 이용하여 첨부된 파일의 검증만을 수행한다.

```js
$(document).ready(function() {
  $('#upFile').fileupload({
    url : '/sample/file/uploadProcessJson.action',
    dataType: 'json',
    replaceFileInput: false,
    add: function(e, data){
      var isValid = true;
      var uploadFile = data.files[0];
      if (!(/png|jpe?g|gif/i).test(uploadFile.name)) {
        alert('png, jpg, gif 만 가능합니다');
        isValid = false;
      } else if (uploadFile.size > 5000000) { // 5mb
        alert('파일 용량은 5메가를 초과할 수 없습니다.');
        isValid = false;
      }

      if (!isValid) {
        // 후처리
      {
    }
  });
});
```
