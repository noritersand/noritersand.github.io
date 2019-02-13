---
layout: post
date: 2016-01-27 02:02:00 +0900
title: 'jQuery: .each()와 jQuery.each()'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - each
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://api.jquery.com/each/](https://api.jquery.com/each/)
- [https://api.jquery.com/jquery.each/](https://api.jquery.com/jquery.each/)

#### .each()

```
$(selector).each( function )
```

설명

#### jQuery.each()

```
jQuery.each( array, callback )
jQuery.each( object, callback )
```

설명

## .each()와 jQuery.each()의 차이

`.each()`와 `jQuery.each()`는 이름 때문에 혼란스럽겠지만 완전히 다른 메서드다. 우선 `.each()`는 Manipulation과 Traversing으로 분류되며 `jQuery.each()`는 Utilities로 분류된다.

아래는 기존에 작성한 설명:

index는 선택한 jQuery 객체 배열의 순번을 의미한다.

```js
$(function() {
  $(document.body).click(function () {
    $("div").each(function (index) {
      if (this.style.color != "blue") {
        $(this).css("color", "blue");
        //this.style.color = "blue";
      } else {
        this.style.color = "";
      }
    });
  });
});
```

each를 사용할 때 주의할 사항은 return으로 메서드를 빠져나갈 수 없다는 것이다.

```html
<h3>jQuery TEST PAGE</h3>
<input type="text" value="first input"/>
<input type="text" value="second input"/>

<script type="text/javascript">
  function eachTest() {
    $('input').each(function(idx) {
      var thisValue = $(this).val();
      if (thisValue.indexOf('first') != -1) {
        return;
        // return true 혹은 return == continue
        // return false == break
      }
      alert($(this).val());
    });    
  }

  eachTest();
</script>
```

아무런 메세지도 뜨지 않고 `eachTest()` 메서드가 종료되길 바라며 위처럼 작성했지만 실제로는 원하는대로 작동하지 않는다. 이것은 `.each()`가 return 혹은 return true를 continue로 받아들이기 때문인데, 원래 의도대로 break를 하고 싶다면 return false를 써야한다.

이에 따라 `.each()`의 콜백함수 내에서의 return은 함수나 메서드의 종료를 의미하지 않는다. 아래 소스를 코멘트가 있을때와 없을 때를 비교해보자.

```html
<h3>jQuery TEST PAGE</h3>
<input type="text" value="first input"/>
<input type="text" value="second input"/>
<script type="text/javascript">
  function eachTest() {
    //var methodExit = false;    
    $('input').each(function(idx) {
      var thisValue = $(this).val();
      if (thisValue.indexOf('first') != -1) {
    //methodExit = true;
        return false;
      }
      alert($(this).val());
    });

    //if (methodExit) {
    //  return;
    //}

    alert('i\'m still alive..');
  }
  eachTest();
</script>
```

```html
<script>
$(document).ready(function () {
  //example 1
  //h3요소 모두 가져오기
  var headers = $('h3');
  // 반복문을 이용한 반복: for문 보다는 jQuery의 each문이 사용하기 편리
  for (var i = 0; i < headers.length; i++) {
    alert($(headers[i]).html());
  }

  $('h3').each(function (index) {
    alert($(this).html());
  });


  //example 2  
  $("div").each(function(i) {
    if (this.style.color != "blue") {
      this.style.color = "blue";
    } else {
      this.style.color = "";
    }
  });

  // example 3
  $("#button1").click(function(event) {   
    var data = [];        
    var form = $("form :input[type=text]");        
    $.each(form, function(e, textBox) {          
      data.push($.trim(textBox.value));        
    });        
    $("#addressData").val(data.join(" "));  

    alert($("#addressData").val());   
  });
});

// example 4 폼 입력값 가져오기
function getAllValue() {
  var address = "";

  $("#address").find("input:text").each(function(i) {
    address += $(this).val();
  });

  alert(address);
}
</script>

<body style="font-size: 9pt;">
<form id="form1">
    <div id="address">
        우편번호 : <input type="text" /> - <input type="text" /> <br />
        주소 : <input type="text" /> <br />
        상세 주소 : <input type="text" /> <br />
        <input  type="button" value="모든 입력 내용값" on_click="getAllValue();" />
        <input id="button1" type="button" value="AllValueToHidden" />
        <input id="addressData" type="hidden" />
    </div>
</form>
```
