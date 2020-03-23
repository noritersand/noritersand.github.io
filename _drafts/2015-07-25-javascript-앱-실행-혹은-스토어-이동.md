---
layout: post
date: 2015-07-25 21:05:00 +0900
title: '[JavaScript] 앱 실행 혹은 스토어 이동'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - launch-app
  - app-store
  - mobile-web
  - 코드모음
  - 비공개
---

* Kramdown table of contents
{:toc .toc}

**비공개 글**

```js
var callApp = {
  isIPHONE: false,
  isIPAD: false,
  isANDROID: false,
  isCHROME: false,
  scheme: '',
  appStoreUrl: '',
  init : function(who) {
    this.isIPHONE = (navigator.userAgent.match('iPhone') != null || navigator.userAgent.match('iPod') != null);
    this.isIPAD = (navigator.userAgent.match('iPad') != null);
    this.isANDROID = (navigator.userAgent.match('Android') != null);
    this.isCHROME = (navigator.userAgent.match('Chrome') != null);
    switch (who) {
    case 'ellotte':
      if (this.isANDROID) {
        this.scheme = 'intent://m.ellotte.com/main.do?cn=152726&cdn=2945814#Intent;scheme=ellotte002;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.ellotte;end';
      } else if (this.isIPHONE || this.isIPAD) {
        this.scheme = 'ellotte001://m.ellotte.com';
        this.appStoreUrl = 'http://itunes.apple.com/kr/app/id902962633?mt=8';
      }
      break;
    case 'smp':
      if (this.isANDROID) {
        this.scheme = 'intent://m.lotte.com/main_smp.do?cn=116824&cdn=601848&smp_yn=Y#Intent;scheme=splotte002a;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.smartpick2a;end'
      } else if (this.isIPHONE || this.isIPAD) {
        this.scheme = 'splotte001://m.lotte.com/main_smp.do?cn=116824&cdn=601848&smp_yn=Y';
        this.appStoreUrl = 'https://itunes.apple.com/app/id483508898';
      }
      break;
    case 'lottedfs':
      if (this.isANDROID) {
        this.scheme = 'intent://#Intent;scheme=lottedutyfree;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.lottedutyfree;end';
      } else if (this.isIPHONE || this.isIPAD) {
        this.scheme = 'lottedfs://m.lottedfs.com';
        this.appStoreUrl = 'https://itunes.apple.com/app/losdemyeonsejeom/id492083651?mt=8';
      }
      break;
    case 'lottesuper':
      if (this.isANDROID) {
        this.scheme = 'intent://order?redirect=http://m.lottesuper.co.kr#Intent;scheme=lottesuper;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lottesuper.mobile;end';
      } else if (this.isIPHONE || this.isIPAD) {
        this.scheme = 'lottesuper://order?redirect=http://m.lottesuper.co.kr';
        this.appStoreUrl = 'https://itunes.apple.com/app/losdemobailsyupeo/id618095243?mt=8';
      }
      break;
    }
  },
  getIframe: function(id, url) {
    var iframe = document.getElementById(id);
    if (iframe !== null) {
      iframe.parentNode.removeChild(iframe);
    }
    iframe = document.createElement('iframe');
    iframe.id = id;
    iframe.style.visibility = 'hidden';
    iframe.style.display = 'none';
    iframe.src = url;
    return iframe;
  },
  exec: function(who, url) {
    this.init(who);
    if (this.isANDROID) { /* 안드로이드 */
      location = this.scheme;        
    } else if (this.isIPHONE || this.isIPAD) { /* IOS */
      setTimeout(function() {
        location = callApp.appStoreUrl;
      }, 500);
      var iframe = this.getIframe('appIframe', this.scheme);
      document.body.appendChild(iframe);
    }
    else { /* 그 외 단말기 */
      location = url;
    }
  }
}

```
