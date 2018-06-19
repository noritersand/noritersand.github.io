---
title: 'JavaScript: Ajax'
date: 2018-03-20 17:08:18
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - ajax
  - xmlhttprequest
---

#### 관련 문서
- https://xhr.spec.whatwg.org/
- http://www.w3schools.com/ajax/
- https://developer.mozilla.org/ko/docs/XMLHttpRequest
- https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest
- http://huns.me/development/1291

## Ajax란?
Ajax는 'Asynchronous JavaScript and XML'의 약자로 웹 애플리케이션 개발 기법의 하나다. Ajax는 독립된 기술을 의미하기보단 여러 기술의 묶음을 지칭하는 용어에 가깝다. 실제로 Ajax를 구현하는 데는 HTML, CSS, DOM, 자바스크립트, XML, XSLT, XPath, XMLHttpRequest 등이 사용된다.

Ajax를 사용하려면 먼저 브라우저의 내장 함수로 XMLHttpRequest 객체(의외지만 XMLHttpRequest는 마이크로소프트에서 처음 만들었다)를 생성해야 한다. 과거엔 ActiveXObject라는 소름 돋는 이름의 생성자를 사용했지만, 웹 사용자의 최소 환경이 IE8인 요즘엔 사용하지 않는다.
```js
var xmlReq = false;
if (window.XMLHttpRequest) { // Non-Microsoft browsers
  xmlReq = new XMLHttpRequest();
} else if (window.ActiveXObject) {
  try {
    xmlReq = new ActiveXObject('Msxml2.XMLHTTP');
    // XMLHttpRequest in later versions of Internet Explorer
  } catch (e1) {
    try {
      xmlReq = new ActiveXObject('Microsoft.XMLHTTP');
      // Try version supported by older versions of Internet Explorer
    } catch (e2) {
      // Unable to create an XMLHttpRequest with ActiveX
    }
  }
}
```
마소가 폭주하던 2000년대에나 쓰이던 코드. 지금은 그냥 아래 한 줄로 해결된다:
```js
var xhr = new XMLHttpRequest();
```
아래 두 예시를 각각 test1.html, test2.html로 만들어서 테스트 해보자:
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>test1.html</title>
<script>
  function loadDoc() {
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4 && xhr.status == 200) {
        document.getElementById('demo').innerHTML = xhr.responseText;
      }
    };
    xhr.open('GET', 'test2.html');
    xhr.send();
  }
</script>
</head>
<body>
<button onclick="loadDoc()">who r u</button>
<div id="demo">
</div>
</body>
</html>
```
```html
<h2>i am waldo</h2>
```

## XMLHttpRequest의 프로퍼티

#### XMLHttpRequest.open()
```
XMLHttpRequest.open( method, url [ , async ] )
```
- **method**: HTTP 메서드 타입 (get/post)
- **url**: 서버 경로
- **async**: 비동기 여부. (true/false)

request의 유형을 지정한다. send()로 메시지를 날리기 전, 어디에 어떤 방식으로 작동하는지를 정하는 메서드. method와 url은 필수지만 async는 기본값이 true로 생략할 수 있는 항목이다. async를 false로 지정할 경우 send() 후 스크립트의 진행을 중단하며 서버로부터 응답이 올 때까지 대기한다.
```js
var xhr = new XMLHttpRequest();
xhr.open('GET', 'test2.html');
// GET 방식으로 요청할 URL 설정
```

#### XMLHttpRequest.setRequestHeader()
```
XMLHttpRequest.setRequestHeader( header, value )
```
- **header**: 헤더의 이름
- **value**: 헤더의 값

HttpRequest 헤더의 값을 설정하는 메서드로 반드시 open()보다 나중에 호출되어야 한다.

#### XMLHttpRequest.send()
```
XMLHttpRequest.send( [ string ] )
```
- **string**: 폼 데이터로 전송할 문자열

서버로 request 송신. open()에서 설정한 값에 따라 서버로 데이터를 요청한다. string은 HTTP 메서드 타입이 POST일 경우에만 명시하며 폼 데이터로 간주된다. POST 방식일 땐 아래 예시처럼 setRequestHeader()로 컨텐츠 타입을 지정하지 않으면 서버에서 처리할 수 없다. 그리고 string은 반드시 쿼리스트링 형태로 작성되어야 한다.
```js
var xhr = new XMLHttpRequest();
xhr.open('POST', 'test4.html');
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
xhr.send('first=111&second=222');
```

#### XMLHttpRequest.abort()
```
xhr.abort();
```
수행 중인 통신을 중단한다. 이미 날라간 request는 어쩔 수 없고 단지 응답을 무시할 뿐. ~~통수~~

#### XMLHttpRequest.getAllResponseHeaders()
response 헤더 정보를 출력한다. send()를 실행하기 전에는 빈 값을 리턴한다. 즉, 서버의 응답 후에만 사용 가능한 메서드
```js
xhr.getAllResponseHeaders();
// "Server: Apache-Coyote/1.1
// Content-Type: text/html;charset=UTF-8
// Content-Length: 22
// Date: Wed, 27 Jan 2016 15:19:02 GMT
// "
```

#### XMLHttpRequest.getResponseHeader()
```
XMLHttpRequest.getResponseHeader( header )
```
- **header**: 헤더의 이름

response 헤더 중 특정한 값만 리턴한다.
```js
xhr.getResponseHeader('Content-Type')
// "text/html;charset=UTF-8"
```

#### XMLHttpRequest.timeout
지정한 시간을 초과하면 XMLHttpRequestEventTarget.ontimeout을 호출한다. 단위는 밀리초(milliseconds)

#### XMLHttpRequest.responseText
서버의 응답을 문자열로 리턴.

#### XMLHttpRequest.responseXML
서버의 응답을 XMLDocument로 파싱해 리턴. 파싱이 불가능하면 null을 리턴한다.

#### XMLHttpRequest.status
응답의 상태코드를 숫자로 리턴한다.

#### XMLHttpRequest.statusText
응답의 상태코드를 문자열로 리턴한다. 단순히 숫자를 문자열로 표현하는게 아니라 상태코드가 뭘 의미하는지 나타낸다. 가령 상태코드가 404일 때 리턴되는 값은 "Not Found"

#### XMLHttpRequest.readyState
XMLHttpRequest의 현재 상태를 의미한다.
- `0`: request not initialized.
- `1`: server connection established
- `2`: request received
- `3`: processing request
- `4`: request finished and response is ready

## Attach callback function
통신 후 성공 혹은 실패에 따른 처리가 필요하다면 서버가 응답했을 때 콜백으로 호출되는 프로퍼티에 함수를 할당하면 된다. 콜백 함수를 할당하는 방법은 XMLHttpRequest의 onreadystatechange() 메서드를 이용하거나 XMLHttpRequestEventTarget의 이벤트 핸들러를 이용하는 방법, 그리고 EventTarget.addEventListener() 이용하는 방법이 있다.

#### XMLHttpRequest.onreadystatechange
```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log('complete');
  }
};
xhr.open('GET', 'http://somewhere');
xhr.send();
```
onreadystatechange()는 XMLHttpRequest의 프로퍼티로 콜백 함수를 저장한다. 이 메서드는 readyState의 값이 변할 때마다 호출되니 그대로 실행되도록 하면 안 되고 readyState의 값을 체크하는 조건 분기를 추가해야 원하는 결과를 얻을 수 있다.

#### [EventTarget.addEventListener()](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)
```
EventTarget.addEventListener( eventType, callback )
```
- **eventType**: callback이 실행될 이벤트의 유형 (ex: 'load')
- **callback**: eventType에 지정한 이벤트가 발생했을 때 실행할 함수

addEventListener()는 IE8에서 지원되지 않으니 주의할 것. ~~악의 축~~
```js
function reqListener () {
  console.log(this.responseText); // this == xhr
}

var xhr = new XMLHttpRequest();
xhr.addEventListener('load', reqListener);
xhr.open('GET', 'http://example.org/example.txt');
xhr.send();
```

#### [XMLHttpRequestEventTarget](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequestEventTarget)
```js
var xhr = new XMLHttpRequest();
console.debug(xhr);
xhr.onabort = function() {
  console.log('onabort - xhr.readyState: ' + xhr.readyState);
};
xhr.onerror = function() {
  console.log('onerror - xhr.readyState: ' + xhr.readyState);
  console.log('xhr.status: ' + xhr.status);
};
xhr.onloadstart = function() {
  console.log('onloadstart - xhr.readyState: ' + xhr.readyState);
};
xhr.onloadend = function() {
  console.log('onloadend - xhr.readyState: ' + xhr.readyState);
};
xhr.onprogress = function(event) {
  console.log('progress - xhr.readyState: ' + xhr.readyState);
  console.log('loaded: ' + event.loaded);
  console.log('size: ' + event.size);
};
xhr.onload = function() {
  console.log('complete');
};
xhr.open('GET', 'test2.html');
xhr.send();
```
XMLHttpRequestEventTarget은 XMLHttpRequest의 슈퍼클래스다. 상속받는 메서드는 다음과 같다:
- `onabort`: abort 이벤트가 발생하면 호출된다.
- `onerror`: 서버의 응답이 200이 아닐 때 호출된다.
- `onloadstart`: XHR 요청을 시작할 때 호출된다.
- `onloadend`: XHR 요청이 완료되면 호출된다. onload()와 다르게 성공/실패 상관없이 호출된다.
- `onprogress`: onloadstart()와 onloadend() 사이에 호출된다. 파라미터로 ProgressEvent를 전달받으며 이 객체의 프로퍼티는 브라우저마다 다를 수 있다.
- `onload`: XHR 요청이 '성공적으로' 완료되면 호출된다. 즉 XHR.status가 200일 때만 호출되는 메서드.
- `ontimeout`: XHR.timeout으로 설정한 시간 내에 응답이 도착하지 않으면 호출된다. 요청은 실패한 것으로 간주되며 onprogress()와 onload()는 호출되지 않는다.
