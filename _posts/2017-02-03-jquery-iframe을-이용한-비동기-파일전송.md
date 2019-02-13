---
layout: post
date: 2017-02-03 13:39:00 +0900
title: 'jQuery: iframe을 이용한 비동기 파일전송'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - iframe
  - file upload
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://hayageek.com/jquery-ajax-form-submit/](http://hayageek.com/jquery-ajax-form-submit/)

#### 브라우저 호환

- 모든 브라우저에서 가능

```html
<script type="text/javascript">
function getDoc(frame) {
  var doc = null;

  // IE8 cascading access check
  try {
    if (frame.contentWindow) {
      doc = frame.contentWindow.document;
    }
  } catch(err) {
  }

  if (doc) { // successful getting content
    return doc;
  }

  try { // simply checking may throw in ie8 under ssl or mismatched protocol
    doc = frame.contentDocument ? frame.contentDocument : frame.document;
  } catch(err) {
    // last attempt
    doc = frame.document;
  }
  return doc;
}

$(document).ready(function() {
  $("#btnSubmit").click(function() {
    var frameName = 'unique' + (new Date().getTime());  //generate a random name

    var $frame = $('<iframe>', {
      src: "javascript: false;",
      name: frameName
    }).appendTo('body').hide();; // Add iframe to body and hide

    var form = document.forms.submitForm;
    form.target = frameName;  //set form target to iframe
    form.action = "/board/notice/adminNoticeDetailPopAjax.do";
    form.submit();

    $frame.load(function(e) {
      var doc = getDoc($frame[0]);
      var docRoot = doc.body ? doc.body : doc.documentElement;
      var data = docRoot.innerHTML;
      //data is returned from server.
      alert($.parseJSON($(docRoot).find("pre").text()).msg);
      close();
    });

    /*
     * 이벤트 .load()는 jQuery 1.8부터 deprecated 되었으므로
     * .ready() 혹은 .on()을 사용할 것
     */
  });
});
</script>
```
