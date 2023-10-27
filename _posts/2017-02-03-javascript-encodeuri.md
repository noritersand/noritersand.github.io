---
layout: post
date: 2017-02-03 13:38:10 +0900
title: '[JavaScript] encodeURI'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - encodeuri
  - querystring
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [URI Generic Syntax RFC 2396](https://www.ietf.org/rfc/rfc2396.txt)
- [UTF-8 encoding table and Unicode characters](https://www.utf8-chartable.de/unicode-utf8-table.pl?start=54528&unicodeinhtml=dec)
- [HTTP/1.1 RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616.html)
- [하이퍼텍스트 전송규약 1.1 표준안](http://coffeenix.net/doc/network/http11.txt)
- [w3schools: HTML URL Encoding Reference](http://www.w3schools.com/tags/ref_urlencode.asp)
- [위키백과: ASCII](https://ko.wikipedia.org/wiki/ASCII)
- [위키백과: 유니코드 A000~AFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_A000~AFFF)
- [위키백과: 유니코드 B000~BFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_B000~BFFF)
- [위키백과: 유니코드 C000~CFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_C000~CFFF)
- [위키백과: 유니코드 D000~DFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_D000~DFFF)


## URI 인코딩이란

```js
https://www.google.co.kr/search?q=%ED%95%9C%EA%B8%80
```

위에서 `%ED%95%9C%EA%B8%80`는 '한글'을 의미한다. 이것은 HTTP 스펙에 따른 변환으로, '한글'을 UTF-8로 인코딩한 뒤 바이트마다 앞에 '%'를 붙여준 값이다. ('한글'은 유니코드 포인트로 `U+D55C U+AE00`이며, UTF-8 인코딩의 16진수 표현으로는 `ed 95 9c ea b8 80`이다.)


## 변환 테스트

<table id="tabKeyCodeTest">
  <tr>
    <td>
      <input id="uri-encode-input" type="text" placeholder="아무 텍스트나 입력해 보세요." onkeyup="keyupHandler()"
          style="width: 500px; border: 0">
    </td>
  </tr>
  <tr>
    <td>
      <textarea id="uri-encode-result" style="width: 500px; height: 100px; border: 0" readonly></textarea>
    </td>
  </tr>
</table>
<script>
function keyupHandler() {
  var value = document.querySelector('#uri-encode-input').value;
  // document.querySelector('#uri-encode-result').textContent = encodeURIComponent(value);
  document.querySelector('#uri-encode-result').value = encodeURIComponent(value);
}
</script>


## encodeURI()와 encodeURIComponent()의 차이

자바스크립트는 `encodeURI()`와 `encodeURIComponent()` 함수를 제공한다. 둘의 차이점은 다음과 같다:

```js
encodeURI('?a=b&c=d');  // "?a=b&c=d"
encodeURIComponent('?a=b&c=d');  // "%3Fa%3Db%26c%3Dd"
```

`encodeURIComponent()`는 인수를 querystring의 일부라고 간주한다. 따라서 `=`, `?`, `&`를 인코딩한다. 반면 `encodeURI()`는 인수를 URI 전체라고 간주하며 파라미터 구분자인 `=`, `?`, `&`를 인코딩하지 않는다.


## 번외: 브라우저 주소창에 보이는 문자 그대로 복사하기

가령 주소가 `https://ko.wikipedia.org/wiki/프로토타입_기반_프로그래밍`인 사이트에 접속 중일 때, 브라우저에서 주소창을 그대로 복사하면 클립보드에는 `https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85_%EA%B8%B0%EB%B0%98_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D` 이렇게 저장된다. 만약 보이는 그대로 복사하고 싶다면 주소의 맨 앞에 스페이스바를 한 칸 넣고 복사하면 된다.
