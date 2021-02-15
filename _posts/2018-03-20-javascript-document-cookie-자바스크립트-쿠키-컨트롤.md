---
layout: post
date: 2018-03-20 16:36:55 +0900
title: '[JavaScript] Document.cookie 자바스크립트 쿠키 컨트롤'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - cookie
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://www.w3schools.com/js/js_cookies.asp](http://www.w3schools.com/js/js_cookies.asp)
- [https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)

## document.cookie

```js
document.cookie = "cookiename=value;"
document.cookie = "cookiename=value; path=/;"
document.cookie = "cookiename=value; path=/; expires=" + new Date()  // 생성과 동시에 만료된다. 즉 삭제
document.cookie = "cookiename=value; path=/; expires=0; domain=.tistory.com"
document.cookie = "cookiename=value; secure"  // HTTPS 전송만 가능
```

#### options

- `;expires`: 쿠키의 만료시간을 의미한다. 명시하지 않거나 잘못된 값을 입력하면 세션쿠키로 생성되서 브라우저 종료 시 삭제된다.
- `;max-age`: `;expires`와 비슷하지만 시각이 아니라 초로 입력한다. (1년이면 31536000초)
- `;domain`: 서버 이름에 따라 쿠키 사용여부가 결정된다. .tistory.com 처럼 메인 도메인명을 지정하면 a.tistory.com, b.tistory.com과 같이 서브 도메인이 달라도 쿠키를 공유한다. 명시하지 않으면 현재 페이지의 location.host값으로 설정된다.
- `;path`: 서버 이름 뒤에 오는 경로에 따라 쿠키 사용여부가 결정된다. 슬래쉬( / )로 설정하면 모든 path에서 공유한다. 명시하지 않으면 현재 페이지의 location.path값으로 설정된다.
- `;secure`: SSL 통신에서만 사용가능한 쿠키가 생성된다. HTTP 문서에서는 접근할 수 없다. 이 옵션을 활성화하지 않는한 HTTP/HTTPS 어느쪽에서 생성한 쿠키든 서로 공유한다.
- `;samesite`: CSRF<sup>사이트간 요청 위조</sup>를 방지하기 위한 옵션. `lax`혹은 `strict`로 설정한다.
    > The `strict` value will prevent the cookie from being sent by the browser to the target site in all cross-site browsing context, even when following a regular link.
    > The `lax` value will only send cookies for TOP LEVEL navigation GET requests. This is sufficient for user tracking, but it will prevent many CSRF attacks.

이 외에 원래 쿠키에는 `HttpOnly`라는 http 전송에만 포함되고 스크립트에서 읽을 수 없게 하는 속성이 있는데 자바스크립트로는 이 속성을 결정할 수 없다.

## 주의사항

쿠키의 값에는 쉼표`,`와 세미콜론`;`을 포함하면 안된다.
http://stackoverflow.com/questions/25387340/is-comma-a-valid-character-in-cookie-value

## examples \#1

```js
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    var expires = "expires="+d.toUTCString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
}

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

## examples \#2

```js
/*\
|*|
|*|  :: cookies.js ::
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
|*|  * docCookies.setItem(name, value[, end[, path[, domain[, secure]]]])
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
  setItem: function(sKey, sValue, vEnd, sPath, sDomain, bSecure) {
    if (!sKey || /^(?:expires|max\-age|path|domain|secure)$/i.test(sKey)) { return false; }
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
```
