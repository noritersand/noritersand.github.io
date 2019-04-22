---
layout: post
date: 2017-02-03 13:38:10 +0900
title: 'JavaScript: encodeURI'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - encodeuri
  - querystring
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.ietf.org/rfc/rfc2396.txt](https://www.ietf.org/rfc/rfc2396.txt)
- [https://www.w3.org/Protocols/rfc2616/rfc2616.html](https://www.w3.org/Protocols/rfc2616/rfc2616.html)
- [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)
- [HTTP 1.1 기술명세서](http://coffeenix.net/doc/network/http11.txt)
- [ASCII 코드표](https://ko.wikipedia.org/wiki/%EB%AF%B8%EA%B5%AD_%EC%A0%95%EB%B3%B4_%EA%B5%90%ED%99%98_%ED%91%9C%EC%A4%80_%EB%B6%80%ED%98%B8)
- [유니코드 A000~AFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_A000~AFFF)
- [유니코드 B000~BFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_B000~BFFF)
- [유니코드 C000~CFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_C000~CFFF)
- [유니코드 D000~DFFF](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_D000~DFFF)

## URI 인코딩이란

```js
https://www.google.co.kr/search?q=%ED%95%9C%EA%B8%80
```

위 `%ED%95%9C%EA%B8%80`는 '한글'을 의미한다. 이것은 HTTP 스펙에 따른 변환으로, '한글'을 UTF-8로 인코딩한 뒤 바이트마다 앞에 '%'를 붙여준 값이다.

## 입력 테스트

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
