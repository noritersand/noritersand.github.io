---
layout: post
date: 2018-03-20 16:36:55 +0900
title: '[JavaScript] document.cookie 자바스크립트 쿠키 컨트롤'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - cookie
  - cookie-util
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [Document Object Model (DOM) Level 2 HTML Specification](https://www.w3.org/TR/DOM-Level-2-HTML/html.html#ID-8747038)
- [http://www.w3schools.com/js/js_cookies.asp](http://www.w3schools.com/js/js_cookies.asp)
- [\[MDN\] Document.cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [모던 JavaScript 튜토리얼: 쿠키와 document.cookie](https://ko.javascript.info/cookie)


## document.cookie

```js
document.cookie = "cookiename=value; samesite=strict"
document.cookie = "cookiename=value; samesite=strict; path=/;"
document.cookie = "cookiename=value; samesite=strict; path=/; expires=" + new Date()  // 생성과 동시에 만료된다. 즉 삭제
document.cookie = "cookiename=value; samesite=strict; path=/; expires=0; domain=.tistory.com"
document.cookie = "cookiename=value; samesite=strict; secure"  // HTTPS 전송만 가능
```

#### options

- `;expires`: 쿠키의 만료시간을 의미한다. 명시하지 않거나 잘못된 값을 입력하면 세션쿠키로 생성되서 브라우저 종료 시 삭제된다.
- `;max-age`: `;expires`와 비슷하지만 시각이 아니라 초로 입력한다. (1년이면 31536000초)
- `;domain`: 서버 이름에 따라 쿠키 사용여부가 결정된다. .tistory.com 처럼 메인 도메인명을 지정하면 a.tistory.com, b.tistory.com과 같이 서브 도메인이 달라도 쿠키를 공유한다. 명시하지 않으면 현재 페이지의 location.host값으로 설정된다.
- `;path`: 서버 이름 뒤에 오는 경로에 따라 쿠키 사용여부가 결정된다. 슬래쉬( / )로 설정하면 모든 path에서 공유한다. 명시하지 않으면 현재 페이지의 location.path값으로 설정된다.
- `;secure`: 값은 없고 이름만 있는 옵션. 이름만 써도 secure 쿠키가 된다. SSL 통신에서만 사용가능한 쿠키가 생성되며, 이 옵션을 활성화하지 않는 한 HTTP/HTTPS 어느 쪽에서 생성한 쿠키든 서로 공유한다.
- `;samesite`: 사이트간 요청 위조(CSRF)를 방지하기 위한 옵션. `lax` 혹은 `strict`, `none`으로 설정한다. `none`일 경우엔 `secure` 옵션이 `true`(라고 써놨지만 사실 `secure`는 언급만 하면 `true`로 설정됨)여야 한다. 명시하지 않으면 브라우저에서 허용하지 않는 경우가 있으니, 그냥 필수값이라고 생각하자.
  > The `strict` value will prevent the cookie from being sent by the browser to the target site in all cross-site browsing context, even when following a regular link.  
  > The `lax` value will only send cookies for TOP LEVEL navigation GET requests. This is sufficient for user tracking, but it will prevent many CSRF attacks.

이 외에 `HttpOnly`라는 HTTP 전송에만 포함되고 스크립트에서 읽을 수 없게 하는 속성이 있는데 자바스크립트로는 이 속성을 결정할 수 없다.


## 주의사항

[쿠키의 값에는 쉼표`,`와 세미콜론`;`을 포함하면 안된다](http://stackoverflow.com/questions/25387340/is-comma-a-valid-character-in-cookie-value).


## examples

### Cook

죠 아래 `docCookies.js`를 ~~요즘 스똬일~~ 클래스로 바꾼 버전.

```js
/*!
 * 쿠키 추가/삭제/조회 프로토타입 생성 파일
 *
 * @author fixalot
 * @since 2023-08-22
 */

/**
 * Cook <br>
 * (괴혈병 없이 세계 일주를 완수한) 제임스 쿡 아님.
 * 원본 작성자: fusionchess from MDN
 * 
 * usages:
 * - Cook.setItem({key: 'foo', value: 'bar', sameSite: 'strict'});
 * - Cook.setItem({key: 'numeric', value: 123, sameSite: 'strict', path: '/', domain: 'localhost'});
 * - Cook.setItem({key: 'secret', value: 'shhhh', sameSite: 'none', secure: true});
 */
class Cook {
  constructor() {}

  static getItem(sKey) {
    if (!sKey) {
      return null;
    }
    return (
      decodeURIComponent(
        document.cookie.replace(new RegExp('(?:(?:^|.*;)\\s*' + encodeURIComponent(sKey).replace(/[\-\.\+\*]/g, '\\$&') + '\\s*\\=\\s*([^;]*).*$)|^.*$'), '$1')
      ) || null
    );
  }

  /**
   * 쿠키 추가
   *
   * @param {Object} obj
   * @param {string} obj.key 추가할 쿠키의 이름
   * @param {string} obj.value 추가할 쿠키의 값
   * @param {string} obj.sameSite SameSite 설정. 허용 범위는 lax|strict|none
   * @param {number|string|Date} obj.expires 쿠키 만료 시간
   * @param {string} obj.path path(도메인 이후의 경로 중 queryString과 hash가 아닌 것)
   * @param {string} obj.domain 도메인
   * @param {boolean} obj.secure secure 쿠키인지
   */
  static setItem({ key: sKey, value: sValue, sameSite: sSamesite, expire: vEnd, path: sPath, domain: sDomain, secure: bSecure }) {
    if (!sSamesite || !/^lax$|^strict$|^none$/.test((sSamesite = sSamesite.toLowerCase()))) {
      console.error('sameSite 값이 없거나 허용 범위가 아닙니다.');
      return false;
    }
    if (!sKey || /^(?:expires|max\-age|path|domain|secure)$/i.test(sKey)) {
      console.error('key 값이 없거나 허용 범위가 아닙니다.');
      return false;
    }
    if (sSamesite.toLowerCase() == 'none') {
      bSecure = true;
    }
    var sExpires = '';
    if (vEnd) {
      switch (vEnd.constructor) {
        case Number:
          sExpires = vEnd === Infinity ? '; expires=Fri, 31 Dec 9999 23:59:59 GMT' : '; max-age=' + vEnd;
          break;
        case String:
          sExpires = '; expires=' + vEnd;
          break;
        case Date:
          sExpires = '; expires=' + vEnd.toUTCString();
          break;
      }
    }
    document.cookie =
      encodeURIComponent(sKey) +
      '=' +
      encodeURIComponent(sValue) +
      '; samesite=' +
      sSamesite +
      sExpires +
      (sDomain ? '; domain=' + sDomain : '') +
      (sPath ? '; path=' + sPath : '') +
      (bSecure ? '; secure' : '');
    return true;
  }

  /**
   * 쿠키 삭제
   *
   * @param {Object} obj
   * @param {string} obj.key 삭제할 쿠키 이름
   * @param {string} obj.path 삭제할 쿠키의 path
   * @param {string} obj.domain 삭제할 쿠키의 도메인
   */
  static removeItem({ key: sKey, path: sPath, domain: sDomain }) {
    if (!this.hasItem(sKey)) {
      console.error('key 값이 없습니다.');
      return false;
    }
    document.cookie =
      encodeURIComponent(sKey) + '=; expires=Thu, 01 Jan 1970 00:00:00 GMT' + (sDomain ? '; domain=' + sDomain : '') + (sPath ? '; path=' + sPath : '');
    return true;
  }

  static hasItem(sKey) {
    if (!sKey) {
      return false;
    }
    return new RegExp('(?:^|;\\s*)' + encodeURIComponent(sKey).replace(/[\-\.\+\*]/g, '\\$&') + '\\s*\\=').test(document.cookie);
  }

  static keys() {
    var aKeys = document.cookie.replace(/((?:^|\s*;)[^\=]+)(?=;|$)|^\s*|\s*(?:\=[^;]*)?(?:\1|$)/g, '').split(/\s*(?:\=[^;]*)?;\s*/);
    for (var nLen = aKeys.length, nIdx = 0; nIdx < nLen; nIdx++) {
      aKeys[nIdx] = decodeURIComponent(aKeys[nIdx]);
    }
    return aKeys;
  }
}
```

### docCookies

```js
/*\
|*|
|*|  :: doc-cookies.js ::
|*|
|*|  A complete cookies reader/writer framework with full unicode support.
|*|
|*|  Revision #1 - September 4, 2014
|*|
|*|  https://developer.mozilla.org/en-US/docs/Web/API/document.cookie
|*|  https://developer.mozilla.org/User:fusionchess
|*|
|*|  This framework is released under the GNU Public License, version 3 or later.
|*|  http://www.gnu.org/licenses/gpl-3.0-standalone.html
|*|
|*|  Syntaxes:
|*|
|*|  * docCookies.setItem(name, value, sameSite[, end[, path[, domain[, secure]]]])
|*|  * docCookies.getItem(name)
|*|  * docCookies.removeItem(name[, path[, domain]])
|*|  * docCookies.hasItem(name)
|*|  * docCookies.keys()
|*|
\*/
var docCookies = {
  getItem: function(sKey) {
    if (!sKey) {
      return null;
    }
    return decodeURIComponent(document.cookie.replace(new RegExp("(?:(?:^|.*;)\\s*"
            + encodeURIComponent(sKey).replace(/[\-\.\+\*]/g, "\\$&")
            + "\\s*\\=\\s*([^;]*).*$)|^.*$"), "$1")) || null;
  },
  setItem: function(sKey, sValue, sSamesite, vEnd, sPath, sDomain, bSecure) {
    if (!sSamesite || !/^lax$|^strict$|^none$/.test(sSamesite = sSamesite.toLowerCase())) { 
      console.error('sameSite 값이 없거나 허용 범위가 아닙니다.')
      return false; 
    }
    if (!sKey || /^(?:expires|max\-age|path|domain|secure)$/i.test(sKey)) { 
      console.error('key 값이 없거나 허용 범위가 아닙니다.')
      return false; 
    }
    var sExpires = "";
    if (vEnd) {
      switch (vEnd.constructor) {
        case Number:
          sExpires = vEnd === Infinity
                  ? "; expires=Fri, 31 Dec 9999 23:59:59 GMT"
                  : "; max-age=" + vEnd;
          break;
        case String:
          sExpires = "; expires=" + vEnd;
          break;
        case Date:
          sExpires = "; expires=" + vEnd.toUTCString();
          break;
      }
    }
    document.cookie = encodeURIComponent(sKey) + "=" + encodeURIComponent(sValue)
            + "; samesite=" + sSamesite
            + sExpires + (sDomain ? "; domain=" + sDomain : "")
            + (sPath ? "; path=" + sPath : "")
            + (bSecure ? "; secure" : "");
    return true;
  },
  removeItem: function(sKey, sPath, sDomain) {
    if (!this.hasItem(sKey)) {
        return false;
    }
    document.cookie = encodeURIComponent(sKey) + "=; expires=Thu, 01 Jan 1970 00:00:00 GMT"
            + (sDomain ? "; domain=" + sDomain : "")
            + (sPath ? "; path=" + sPath : "");
    return true;
  },
  hasItem: function(sKey) {
    if (!sKey) {
        return false;
    }
    return (new RegExp("(?:^|;\\s*)" + encodeURIComponent(sKey).replace(/[\-\.\+\*]/g, "\\$&")
        + "\\s*\\=")).test(document.cookie);
  },
  keys: function() {
    var aKeys = document.cookie.replace(/((?:^|\s*;)[^\=]+)(?=;|$)|^\s*|\s*(?:\=[^;]*)?(?:\1|$)/g, "")
            .split(/\s*(?:\=[^;]*)?;\s*/);
    for (var nLen = aKeys.length, nIdx = 0; nIdx < nLen; nIdx++) {
        aKeys[nIdx] = decodeURIComponent(aKeys[nIdx]);
    }
    return aKeys;
  }
};

docCookies.setItem('foo', 'bar', 'strict');
```

### simple getter/setter

```js
function getCookie(cname) {
    var name = cname + "=";
    var ca = document.cookie.split(';');
    for (var i = 0; i < ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1);
        if (c.indexOf(name) == 0) return c.substring(name.length, c.length);
    }
    return "";
}

function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    var expires = "expires="+d.toUTCString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
}

function checkCookie() {
    var user = getCookie("username");
    if (user != "") {
        alert("Welcome again " + user);
    } else {
        user = prompt("Please enter your name:", "");
        if (user != "" && user != null) {
            setCookie("username", user, 365);
        }
    }
}
```
