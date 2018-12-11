---
layout: post
date: 2017-02-17 11:41:00 +0900
title: 'JavaScript: IE7, IE8에 없는 함수 추가'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - ie
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```js
/**
 * 브라우저 판단
 */
var browser = {
    ie7: (!!window.navigator.userAgent.match(/MSIE 7.0/)) ? true : false,
    ie8: (!!window.navigator.userAgent.match(/MSIE 8.0/)) ? true : false
};

/**
 * Array 프로토타입의 메서드로 map 추가(IE7, 8에 Array.map 없음)
 */
if (!('map' in Array.prototype)) {
    Array.prototype.map = function(mapper, that) {
        var other = new Array(this.length);
        for (var i = 0, n = this.length; i < n; i++) {
            if (i in this) {
                other[i] = mapper.call(that, this[i], i, this);
            }
        }
        return other;
    };
}

/**
 * String 프로토타입 메서드로 trim 추가(IE7, 8에 String.trim 없음)
 */
if (typeof String.prototype.trim !== 'function') {
  String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g, '');
  }
}

/**
 * jQuery 확장 메서드로 stringify 추가. (IE7에 JSON 없음)
 */
$.extend({
  stringify: function stringify(obj) {
    if (window.JSON && window.JSON.stringify) {
      return JSON.stringify(obj);
    }
    var t = typeof (obj);
    if (t != "object" || obj === null) {
      // simple data type
      if (t == "string") obj = '"' + obj + '"';
      return String(obj);
    } else {
      // recurse array or object
      var n, v, json = [], arr = (obj && obj.constructor == Array);

      for (n in obj) {
        v = obj[n];
        t = typeof(v);
        if (obj.hasOwnProperty(n)) {
          if (t == "string") v = '"' + v + '"';
          else if (t == "object" && v !== null) v = jQuery.stringify(v);
          json.push((arr ? "" : '"' + n + '":') + String(v));
        }
      }
      return (arr ? "[" : "{") + String(json) + (arr ? "]" : "}");
    }
  }
});
```
