---
layout: post
date: 2021-02-20 11:24:05 +0900
title: '[JavaScript] URL에서 특정 파라미터 추출 parameterUtil'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - parameter
  - parameter-util
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

URL의 쿼리스트링에서 특정 파라미터를 추출하는 스크립트.

```js
let parameterUtil = {
  /**
   * URL의 쿼리스트링에서 name을 추출해 반환.
   *
   * @param {string} name url에서 추출할 파라미터의 이름
   * @param {string} url 생략하면 현재 페이지의 주소를 사용함
   * @returns {string} 파라미터의 값
   */
  getByName: function(name, url) {
    if (!url) {
      url = window.location.href;
    }
    name = name.replace(/[\[\]]/g, '\\$&');
    var regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)'),
      results = regex.exec(url);
    if (!results) {
      return null;
    }
    if (!results[2]) {
      return '';
    }
    return decodeURIComponent(results[2].replace(/\+/g, ' '));
  }
}
```

어디서 퍼온 건지, 내가 만든 건지(정규식을 보니 그럴리는 없고...), 아니면 이것 저것 모아 짜깁기한 건지 기억도 안 나는 코드다. 😑

이렇게 쓺:

```js
var url = 'https://www.google.com/search?newwindow=1&client=firefox-b-d&ei=gNWhXPP-KISl8QWCtJDYDA&q=javascript+replace&oq=javascript+replace';

var client = parameterUtil.getByName('client', url);
console.log(client); // firefox-b-d

var q = parameterUtil.getByName('q', url);
console.log(q); // javascript replace
```
