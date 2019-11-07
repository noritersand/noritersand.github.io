---
layout: post
date: 2017-02-03 13:38:10 +0900
title: '[JavaScript] Window.opener'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - window.opener
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.w3schools.com/jsref/prop_win_opener.asp](http://www.w3schools.com/jsref/prop_win_opener.asp)
- [https://developer.mozilla.org/en-US/docs/Web/API/Window.opener](https://developer.mozilla.org/en-US/docs/Web/API/Window.opener)

## opener

```js
window.opener.document
```

`window.open()`으로 윈도우 객체가 생성될 때 자바스크립트는 window.opener 프로퍼티에 윈도우를 연 객체(부모)를 저장한다. 이를 이용하면 자식 창에서 부모 창을 컨트롤하거나 서로간 데이터를 주고받는게 가능하다.

## example

간단한 예를 통해 아라보자:

### test.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  var newWindow;

  function openNewWindow(){
    newWindow = window.open("test2.html", "newWindow");
  }

  function recieve(){
    var txt = "<font color='red'>자식창에서 받아온 값</font>";
    document.getElementById("process").innerHTML = txt;
    document.myform.receiver.value = newWindow.document.myform.sender.value;
  }
</script>
</head>
<body>
<form name="myform">
  <input type="button" value="자식창 열기" onclick="openNewWindow()"><br>
  부모창 Sender : <input type="text" name="sender" size="10"><br>
  부모창 Receiver : <input type="text" name="receiver" size="10"><span id="process"></span><br/>
  <input type="button" value="받아오기" onclick="recieve()">
</form>
</body>
</html>
```

### test2.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  function recieve(){
    var txt = "<font color='red'>부모창에서 받아온 값</font>";
    document.getElementById("process").innerHTML = txt;
    document.myform.receiver.value = window.opener.document.myform.sender.value;
  }

  function send() {
    var txt = "<font color='blue'>자식창에서 보낸 값</font>";
    window.opener.document.myform.receiver.value = document.myform.sender.value;
    window.opener.document.getElementById("process").innerHTML = txt;
    window.close();
  }
</script>
</head>
<body>
<form name="myform">
  자식창 Sender : <input type="text" name="sender" size="10"><br>
  자식창 Receiver : <input type="text" name="receiver" size="10"><span id="process"></span><br/>
  <input type="button" value="받아오기" onclick="recieve()">
  <input type="button" value="부모창에 데이터 보내고 닫기" onclick="send()"/>
</form>
</body>
</html>
```

반대로 부모 창에서 자식 창의 문서에 접근할 땐 window.open() 함수가 반환하는 객체를 이용한다:

```js
var winObj = window.open();
```

만약 부모 창과 데이터를 주고 받을 자식 창이 여러 페이지에서 공통으로 사용되는 경우(공통팝업)라면, 자식 창에서 부모창을 직접 조작하는것보다 부모창의 함수를 호출하여 파라미터로 던져주는 방식을 써야한다. (혹은 부모의 함수명을 콜백으로 전달하는 방식을 쓰던지..)

### test3.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  var newWindow;
  function openNewWindow(){
    newWindow = window.open("test4.html", "newWindow");
  }
  function sendMeData(data) {
    var txt = "<font color='red'>자식창에서 받아온 값 : </font>";
    document.getElementById("process").innerHTML = txt;
    document.getElementById("result").innerHTML = data;
  }
</script>
</head>
<body>
<input type="button" value="자식창 열기" onclick="openNewWindow()"><br>
<span id="process"></span><span id="result"></span>
</body>
</html>
```

### test4.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  function send() {
    var data = document.myform.sender.value;
    window.opener.sendMeData(data)
    window.close();
  }
</script>
</head>
<body>
<form name="myform">
  자식창 Sender : <input type="text" name="sender" size="10"><br>
  <input type="button" value="부모창에 데이터 보내고 닫기" onclick="send()"/>
</form>
</body>
</html>
```
