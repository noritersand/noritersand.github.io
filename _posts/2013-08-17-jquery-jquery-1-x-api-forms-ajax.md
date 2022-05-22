---
layout: post
date: 2013-08-17 05:00:26 +0900
title: '[jQuery] jQuery-1.x API: Forms, Ajax'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - ajax
  - forms
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://api.jquery.com/category/forms](https://api.jquery.com/category/forms)
- [https://api.jquery.com/category/ajax](https://api.jquery.com/category/ajax)

## Forms

### .blur()

blur 이벤트를 실행한다. blur 이벤트는 요소가 마우스나 키보드 등의 컨트롤러에 의해 포커스를 잃을 때 발생

### .blur( handler )

blur 이벤트에 실행할 함수를 할당한다.

### .change()

change 이벤트를 실행한다. 포커스를 잃은 상태의 input 요소가 포커스를 얻고, 값의 변경을 완료했을 때에 실행된다.

### .change( handler )

change 이벤트에 실행할 함수를 할당한다.

### .focus()

focus 이벤트를 발생시킨다.

```js
// 엔터치면 다음으로 이동
$(function(){
  $('input').not($(':button')).keypress(function (evt) {
    if (evt.keyCode == 13) {
      var fields = $(this).parents('form:eq(0),body').find('button,input,select');
      var index = fields.index(this);
      if (index > -1 && (index + 1) < fields.length) {
        fields.eq(index + 1).focus();
      }
      return false;
    }
  });
});
```

### .focus( handler )

focus 이벤트에 실행할 함수를 할당한다.

### $.param( object )

JSON으로 표현 가능한 객체를 HTTP query string 형식으로 문자화한다.

```js
$.param([
  { name: "first", value: "Rick" },
  { name: "last", value: "Astley" },
  { name: "job", value: "Rock Star" }
]);
// "first=Rick&last=Astley&job=Rock+Star"

$.param({
  cht: "qr", choe: "UTF-8", chs: "300x300", chld: "L|0", chl: 'some-values'
});
// "cht=qr&choe=UTF-8&chs=300x300&chld=L%7C0&chl=some-values"
```

### .select()

select 이벤트를 발생시킨다. select 이벤트는 텍스트 에리어의 문자열을 선택 상태로 하거나 선택 범위를 변경했을 때에 실행 한다.

### .select( handler )

select 이벤트에 실행할 함수를 할당한다.

### .serialize(), .serializeArray()

`serialize()`는 지정된 폼에 속한 모든 입력필드(input, textarea, select가 해당됨. 단, name 속성과 값이 반드시 존재해야 함)의 값을 HTTP query string 형식으로 반환한다:

```html
<form>
  <input type="checkbox" name="checkbox1" checked value="11"><br>
  <input type="text" name="textInput" value="22"><br>
  <select name="selected">
    <option selected value="33">33</option>
  </select>
</form>
```

```js
$('form').serialize();
// checkbox1=11&textInput=22&selected=33
```

`.serializeArray()`는 문자열이 아니라 JS 객체 배열로 반환한다:

```js
$('form').serializeArray();
// Array(3) [ {…}, {…}, {…} ]
//   0: Object { name: "checkbox1", value: "11" }
//   1: Object { name: "textInput", value: "22" }
//   2: Object { name: "selected", value: "33" }
```

그런데 영 쓸대가 없다.

대신 [jquery-serialize-object](https://github.com/macek/jquery-serialize-object)를 써보자.

### .submit()

submission 이벤트를 발생시킨다.

### .submit( handler )

submission 이벤트에 실행할 함수를 할당한다.

### .val()

모든 요소의 value 속성을 반환한다.

### .val( val )

모든 요소의value 속성에 값을 지정한다.

## Ajax

### jQuery.ajax()

```
jQuery.ajax( [settings ] )
jQuery.ajax( url [, settings ] )
```

- `url`: String, 요청 URL
- `method`: 사용할 HTTP 메서드. GET, POST, PUT 중에 선택한다. default는 GET이며 1.9.0에서 추가된 옵션으로 이전 버전의 type을 대체한다.
- `type`: method의 별칭. 1.9.0 이전 버전에서 사용한다.
- `data`: Object, 요청에 전달되는 프로퍼티를 가진 객체
- `dataType`: String, 응답의 결과로 반환되는 데이터의 종류를 식별하는 키워드. 필요하다면 이 값을 통해 데이터를 콜백 함수로 전달하기 전에 어떤 종류의 후처리가 발생할지 결정한다. "xml", "html", "json", "jsonp", "script", "text" 등으로 설정 가능.
- `contentType`: 서버에 데이터를 보낼 때 사용되는 content-type이며 기본값은 'application/x-www-form-urlencoded; charset=UTF-8'. 케릭터 셋인 UTF-8은 변경할 수 없다고 한다.[^1] false로 설정하면 어떤 content-type을 사용할 지 알리지 않는다.
- `timeout`: Number, Ajax 요청의 제한 시간을 밀리초 단위로 설정한다. 제한 시간 안에 요청이 완료되지 않으면 요청을 취소하거나, error 콜백이 정의되어 있다면 호출된다.
- `global`: Boolean, true나 false에 따라 전역 함수를 활성화하거나 비활성화한다. 전역 함수는 엘리먼트에 덧붙일 수 있으며 Ajax 호출 동안 다양한 위치나 조건에서 실행된다. contentType String 요청에 명시되는 콘덴츠 타입. 생략하면 'application/x-www-form-urlencoded'가 기본으로 설정된다.
- `success`: Function, 응답이 성공 상태 코드를 반환하면 호출되는 함수. 응답 본문은 이 함수의 첫 번째 매개변수로 전달되며, dataType 프로퍼티에 명시한 형태로 구성된다. 두번째 매개변수는 상태 값을 나타내는 문자열이며 이번 경우에는 항상 'success'다
- `error`: Function 응답이 에러 상태 코드를 반환하면 호출되는 함수. 매개변수가 세 개 전달되는데, 각각 XHR 인스턴스, 상태 값이 항상 'error'인 메시지 문자열, 선택사항으로 XHR 인스턴스가 반환한 예외 객체이다.
- `complete`: Function 요청이 완료되면 호출되는 함수. 매개변수 두 개가 전달되는데, 각각 XHR 인스턴스와 'success'혹은 'error'를 나타내는 상태 메시지 문자열이다. success나 error 콜백을 명시했다면, 이 함수는 해당 콜백이 호출된 후에 실행된다.
- `beforeSend`: Function, 요청이 전송되기에 앞서 먼저 호출되는 함수. 이 함수는 XHR 인스턴스를 전달 받으며, 사용자 정의 헤더를 설정하거나 요청 전에 필요한 연산을 수행하는 데 사용할 수 있다.
- `async`: Boolean, false면 요청이 동기 호출로 전송된다. 기본은 비동기 요청이다.
- `processData`: Boolean, 기본값인 true일 때 객체로 전달된 데이터를 쿼리 문자열로 변환한다. FormData 등 쿼리 문자열 변환이 불가능한 -비 처리된- 데이터를 전달할 때는 false로 설정한다.
- `ifModified`: Boolean, true일 때 Last-Modified 헤더를 확인하여 마지막 요청 이후에 응답 콘텐츠가 변경되지 않았다면 요청이 성공한다. 만일 생략하면 헤더를 확인하지 않는다.
- 이 외 항목은 다음 링크 참고: [https://api.jquery.com/jquery.ajax/#jQuery-ajax-settings](https://api.jquery.com/jquery.ajax/#jQuery-ajax-settings)

```js
var num1 = $('#num1').val();
var num2 = $('#num2').val();
var o = $('#oper').val();
var url = 'test4_ok.jsp';
var params = 'num1=' + num1 + '&num2=' + num2 + '&oper=' + o;

$.ajax({
  method: 'post',
  url: url,
  data: params,
  contentType: 'application/json; charset=utf-8', // 전송 데이터 타입
  dataType: 'json', // 응답 데이터 타입
  beforeSend: showRequest, //ajax로 전송 전 호출할 함수
  success: function(data){
    $('#result').html(data);
  },
  error: function(e){
    alert(e.responseText);
  }
});
```

success 옵션을 다음처럼 작성하면 응답값을 자세히 확인할 수 있다:

```js
success: function(responseData, textStatus, jqXHR) {
  console.log(responseData);
  console.log(textStatus);
  console.log(jqXHR.status);
}
```

마찬가지로 실패 시 응답값을 자세히 보려면 error 옵션을 다음처럼 작성한다:

```js
error: function(jqXHR, textStatus, errorThrown) {
  var errorMsg = 'status(code): ' + jqXHR.status + '\n';
  errorMsg += 'statusText: ' + jqXHR.statusText + '\n';
  errorMsg += 'responseText: ' + jqXHR.responseText + '\n';
  errorMsg += 'textStatus: ' + textStatus + '\n';
  errorMsg += 'errorThrown: ' + errorThrown;
  alert(errorMsg);
}
```

### .load()

HTML을 읽어들여, DOM에 삽입 한다.

```
jQuery( selector ).load( url [, data ] [, complete ] )
```

- `selector`: 응답된 결과를 삽입할 요소. 여는 태그와 닫는 태그가 쌍으로 존재하는 요소를 선택해야 정상 작동 한다. (div, span 같은...)
- `url`: 서버 url
- `data`: request로 전달할 데이터
- `complete`: 요청 완료 후 수행할 콜백 함수

```js
$('#success').load('demo.jsp', function(response, status, xhr) {
  if (status == 'error') {
    alert(xhr.status + ' ' + xhr.statusText);
  }
});

$('#success').load('demo.jsp', { "choices[]": ["han", "lee"] } );

$('#success').load('demo.jsp', {limit: 25}, function(){
  alert('Test~!');
});
```

### jQuery.get()

매개변수로 명시된 URL을 사용하여 서버에 GET 요청을 전송한다.

```
jQuery.get( [settings ] )
jQuery.get( url [, data ] [, success ] [, dataType ] )
```

- `url`: 요청할 URL
- `data`: request로 전달할 데이터
- `success`: 요청 완료 후 수행할 콜백 함수
- `dataType`: 응답 데이터의 타입을 지정한다. (xml, json, script, html)

```js
//get 방식(responseText)
$.get(url, { num1: num1, num2: num2, oper: o }, function(data){
    $('#result').html(data);
});
```

### jQuery.getJSON()

파라미터로 명시된 URL을 사용하여 서버에 GET 요청을 전송한다. 응답 데이터는 JSON 형식이어야 한다.

```
jQuery.getJSON( url [, data ] [, success ] )
```

- `url`: 서버 url
- `data`: request로 전달할 데이터
- `success`: 요청 완료 후 수행할 콜백 함수

```js
$.getJSON('testJSON.js', function (data) {
  $('#pnlDisplay').empty(); //패널(div)의 내용 초기화
  var table = '<table border='1'><tr><td>인덱스</td><td>번호</td><td>이름</td></tr>';
  //data를 탐색:each()메서드를 사용해서 데이터가 있는 만큼 반복
  $.each(data, function (index, entity) {
    table += '<tr>';
    table += '<td>' + index + '</td>';
    table += '<td>' + entity['Num'] + '</td>';
    if (entity['Name']) { //특정필드를 비교할때 이러한 표현법
      table += '<td>' + entity['Name'] + '</td>';
    }
    table += '</tr>';
  });
  table += '</table>';
  $('#pnlDisplay').append(table); //패널에 추가하기
});
```

### jQuery.getScript()

매개변수로 명시된 URL을 사용하여 서버에 GET 요청을 전송한다. 응답 데이터는 JavaScript 파일 형식이어야 한다.

```
jQuery.getScript( url [, success ] )
```

- `url`: 서버 url
- `success`: 요청 완료 후 수행할 콜백 함수

```js
$('#btn').click(function () {
  //에러 코드가 실행되는 시점에 js파일의 기능 실행됨
  $.getScript('testScript.js');
});
```

### jQuery.post()

매개변수로 명시된 URL을 사용하여 서버에 POST 요청을 전송한다.

```
jQuery.post( url [, data ] [, success ] [, dataType ] )
jQuery.post( [settings ] )
```

- `url`: 서버 url
- `data`: request로 전달할 데이터
- `success`: 요청 완료 후 수행할 콜백 함수
- `dataType`: 응답 데이터의 타입을 지정한다. (xml, json, script, html)

```js
$.post(url, { num1: num1, num2: num2, oper: o }, function(data){
    $('#result').html(data);
});
```

## Local ajax event handlers

`jQuery.ajax()`와 그 파생 메서드들은 jQuery-XMLHttpRequest Object를 반환하는데, 이를 통해 수신된 결과를 처리하도록 핸들러를 등록할 수 잇다.

```js
var jqxhr = $.post('example.php', function() {
    alert('success');
});

jqxhr.fail(function() {
    alert('error');
});
```

그리고 done, fail, always 등의 Ajax 이벤트 관련 메서드는 항상 this를 반환한다는 점을 이용하여 다음처럼 콜체인 형식으로 작성할 수도 있다.

```js
$.get('example.php', function() {
    alert('success');
}).done(function() {
    alert('second success');
}).fail(function() {
    alert('error');
}).always(function() {
    alert('finished');
});
```

### Deprecation Notice

> The jqXHR.success(), jqXHR.error(), and jqXHR.complete() callback methods introduced in jQuery 1.5 are deprecated as of jQuery 1.8. To prepare your code for their eventual removal, use jqXHR.done(), jqXHR.fail(), and jqXHR.always() instead.
>
>[https://api.jquery.com/jQuery.Ajax/#jqXHR](https://api.jquery.com/jQuery.Ajax/#jqXHR)

`jqXHR.success()`, `jqXHR.error()`, `jqXHR.complete()`는 1.8 버전 이후로 사용이 권장되지 않는다. (1.11 버전 기준 아직 삭제되진 않았음)

혼동될 수 있는게 폐기 예정인 것은 `ajax()`의 옵션이 아니라 jqXHR의 메서드다. `ajax()`의 옵션[^3]인 success, error, complete는 기존 그대로 사용한다. `ajax()`와 그 파생형인 `get()`, `post()` 등이 반환하는 jqXHR 객체의 메서드인 success/done, complete/always, error/fail 중에서 success, error, complete가 deprecation 된 것이다.

아래는 권장사항을 준수한 예시:

```js
$.ajax({
    method: 'post',
    url: '/member/updatePassword.do',
    data: 'memberNumber=65536',
    beforeSend: function() {
      console.log('before send');
    },
    success: function() {
        console.log('success');
    },
    error: function() {
        console.log('error');
    },
    complete: function() {
        console.log('complete');
    }
}).done(function() {
    console.log('done');
}).fail(function() {
    console.log('fail');
}).always(function() {
    console.log('always');
});
```

## Global ajax event handlers

아래 나열한 메서드들은 전역으로 설정가능한 ajax 이벤트 핸들러로 1.9.x 버전부터는 document에만 할당 할 수 있도록 변경되었다. 핸들러를 할당하고 난 뒤에는 문서에서 발생한 모든 Ajax 이벤트에 반응한다.

- `.ajaxStart( callback( jqEvent ) )`: Ajax request의 송신 전 active request가 없는 경우에 실행되는 함수를 지정한다.
- `.ajaxSend( callback( jqEvent, jqXHR, ajaxOptions ) )`: Ajax request의 송신 전 실행되는 함수를 지정한다.
- `.ajaxSuccess( callback( jqEvent, jqXHR, ajaxOptions, data ) )`: Ajax request의 송신 성공 시 실행되는 함수를 지정한다.
- `.ajaxError( callback( jqEvent, jqXHR, ajaxSettings, thrownError ) )`: Ajax request의 송신 실패 시 실행되는 함수를 지정한다.
- `.ajaxComplete( callback( jqEvent, jqXHR, ajaxOptions ) )`: Ajax request의 송신 완료 시 실행되는 함수를 지정한다.
- `.ajaxStop( callback )`: Ajax request의 송신 종료 시 실행되는 함수를 지정한다.

```js
$(document).ajaxStart(function() {
  console.log('ajax stated');
}).ajaxComplete(function(){
  console.log('ajax complete');
});
```

[^1]: Note: The W3C XMLHttpRequest specification dictates that the charset is always UTF-8; specifying another charset will not force the browser to change the encoding
[^3]: 함수의 인수로 전달하는 JSON 객체의 프로퍼티. `$.ajax({url:'...'})`에서 url은 `ajax()`의 옵션이다.
