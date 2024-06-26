---
layout: post
date: 2017-02-03 13:38:10 +0900
title: '[Web API] Window.open(), Window.opener'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - html-standard
  - window
  - open
  - opener
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Window.open() \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/open)
- [https://developer.mozilla.org/en-US/docs/Web/API/Window.opener](https://developer.mozilla.org/en-US/docs/Web/API/Window.opener)


## 개요

인스턴스 메서드 `Window.open()`과 인스턴스 프로퍼티 `Window.opener` 정리


## open()

```
window.open( URL, name [ , specs ] [ , replace ] )
```

- `URL`: 문서의 URL
- `name`: URL이 로드될 창의 이름을 지정하거나 기존의 창 이름을 지정할 경우 해당 윈도우를 재사용한다.
- `specs`: 창의 위치와 크기, 기능 등 창이 갖는 특성을 지정한다. 이 값은 브라우저/버전마다 적용이 되기도 하고 무시되기도 한다. 가령 `location`은 과거엔(IE 구버전에선 아직도 적용됨) 숨길 수 있었으나, 최근엔 대부분의 브라우저에서 보안상의 이유로 무시하는 값이다.
- `replace`: true일 땐 새 문서가 이전의 문서와 교체된다. false이거나 지정되지 않으면 새 문서는 창의 브라우징 히스토리에 새 항목으로 추가된다.

```js
function openNewWindow(url, name) {
  var specs = "left=10,top=10,width=372,height=466";
  specs += ",menubar=no,toolbar=yes,location=no,status=yes,titlebar=no,scrollbars=yes,resizable=yes";
  window.open(url, name, specs);
}
```

**name**: 연결된 문서를 읽어 지정한 이름의 윈도우나 프레임(frame)을 target으로 설정한다. 지정된 이름을 가진 윈도우가 없으면 새 창으로 처리된다.

- `_blank`: 연결된 문서를 읽어 새로운 빈 윈도우에 표시한다. 윈도우 이름은 없다.
- `_media`: 연결된 문서를 읽어 미디어바의 HTML 내용부분에 표시한다. IE6부터 적용된다.
- `_parent`: 연결된 문서를 읽어 바로 상위 모체창에 표시한다.
- `_search`: 연결된 문서를 읽어 브라우저의 검색창에 표시한다. IE5부터 적용된다.
- `_self`: 디폴트이며, 연결된 문서를 읽어 현재창에 표시한다.
- `_top`: 연결된 문서를 읽어 최상위 윈도우에 표시한다.

URL과 name만 명시하면 대부분의 브라우저에서 새 탭으로 처리된다. 만약 단순 새 탭이나 새 창이 아닌 '팝업'이라 통용되는 창을 생성하려 한다면 사이즈(width, height)를 명시해야 한다:

```js
window.open("/test.jsp", "팝업", "left=10, top=10, width=500, height=500");
```

화면 정중앙에 새 창을 띄우려면 다음처럼 부모창의 window 객체가 반환하는 사이즈 값을 이용해 처리한다.

```js
var width=800, height=500;
var left = (screen.availWidth - width)/2;
var top = (screen.availHeight - height)/2;
var specs = "width=" + width;
specs += ",height=" + height;
specs += ",left=" + left;
specs += ",top=" + top;

window.open("/test.jsp", "팝업", specs);
```

### 브라우저별 특성

#### 크롬

새 창 옵션 중 menubar가 yes일 경우 크롬은 새 창 팝업 대신 새 탭 팝업으로 열린다.

#### 파이어폭스

**TODO**

#### 엣지

**TODO**


## opener

```js
window.opener.document
```

`window.open()`으로 윈도우 객체가 생성될 때 자바스크립트는 window.opener 프로퍼티에 윈도우를 연 객체(부모)를 저장한다. 이를 이용하면 자식 창에서 부모 창을 컨트롤하거나 서로간 데이터를 주고받는게 가능하다.

### example

간단한 예시를 통해 araboja:

#### test.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  var newWindow;

  function openNewWindow() {
    newWindow = window.open("test2.html", "newWindow");
  }

  function recieve() {
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

#### test2.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  function recieve() {
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

#### test3.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
  var newWindow;
  function openNewWindow() {
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

#### test4.html

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

### window.opener에 권한 문제로 접근할 수 없는 경우 해결방법

```js
opener.name;
"액세스가 거부되었습니다." // 익스(사망)
SecurityError: Blocked a frame with origin "http://www.daum.net" from accessing a cross-origin frame // 초로미
Error: Permission denied to access property 'name' // 불여시
```

window.open()으로 생성된 새 창(자식 창)에서 부모 창에 접근할 수 없는 경우가 있는데, 이것은 보통 부모 창과 자식 창의 호스트명이나 프로토콜, 포트가 다를때 [동일 출처 정책(same-origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)에 따라 브라우저가 접근을 차단하는 경우다.

이를 해결하는 방법은 두 가지가 있다.


#### document.domain 변경

부모 창의 도메인은 'abc.tistory.com'이고 자식 창의 도메인은 'def.tistory.com'일 때 두 창의 도메인을 모두 'tistory.com'으로 변경한다. 도메인은 원하는 값으로 마음대로 변경할 수 있는건 아니고 서브도메인을 삭제하는것만 허용된다. 가령 실제 도메인이 'noritersand.tistory.com'일 때 변경 가능한 값은 'noritersand.tistory.com' 혹은 'tistory.com'이다.

```js
document.domain; // 'noritersand.tistory.com'
document.domain = 'aa.tistory.com'; // Error: Illegal document.domain value
document.domain = 'tistory.com'; // 'tistory.com'
document.domain = 'daum.net'; // Error: Illegal document.domain value
```


#### 부모 창과 같은 도메인으로 이동

먼저 open 함수를 사용하여 다른 도메인의 자식 창을 생성한다.

![](/images/window-opener-1.png)

앞서 말했다시피 도메인이 다르기 때문에 자식 창에선 부모 창에 접근할 수 없다. 따라서 자식 창의 페이지를 부모와 같은 도메인의 페이지로 변경한다. (SUBMIT 혹은 location.href)

![](/images/window-opener-2.png)

이제 도메인이 같아졌으므로 opener를 통해 부모 창 객체에 접근할 수 있다.

![](/images/window-opener-3.png)
