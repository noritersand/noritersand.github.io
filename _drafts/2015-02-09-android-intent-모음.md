---
layout: post
date: 2015-02-09 13:24:00 +0900
title: '[Android] intent 모음'
categories:
  - android
tags:
  - android
  - intent
  - code-snippet
  - 비공개
---

**비공개 글**

#### 참고한 문서

- [https://developer.chrome.com/multidevice/android/intents](https://developer.chrome.com/multidevice/android/intents)

## syntax

```js
intent:
   HOST/URI-path // Optional host
   #Intent;
      package=[string];
      action=[string];
      category=[string];
      component=[string];
      scheme=[string];
   end;
```

```js
intent://#Intent;scheme=스킴명;package=패키지명;end
```

## intent 모음

- MISP: `intent://TID=#Intent;scheme=ispmobile;package=kvp.jjy.MispAndroid320;end`
- 엘롯데: `intent://m.ellotte.com/main.do?cn=152726&cdn=2945814#Intent;scheme=ellotte002;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.ellotte;end`
- 롯데닷컴: `intent://m.lotte.com/main_smp.do?cn=116824&cdn=601848&smp_yn=Y#Intent;scheme=splotte002a;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.smartpick2a;end`
- 롯데면세점: `intent://#Intent;scheme=lottedutyfree;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lotte.lottedutyfree;end`
- 롯데슈퍼: `intent://order?redirect=http://m.lottesuper.co.kr#Intent;scheme=lottesuper;action=android.intent.action.VIEW;category=android.intent.category.BROWSABLE;package=com.lottesuper.mobile;end`
- 위챗: `intent://#Intent;scheme=weixin;package=com.tencent.mm;end`
- 위챗-QR코드 스캔: `intent://dl/scan#Intent;scheme=weixin;package=com.tencent.mm;end`
- 위챗 스토어로 보내기: http://www.wechat.com/cgi-bin/download302?fr=wechat.com&url=androidMarket&cl=mobile

## 여담

### 사이트 주소 링크로 모바일 앱 실행하기

특정 사이트의 링크를 실행하려고 할 때 설치되어 있는 앱들의 '지원하는 웹 주소' 값을 이용하는 OS 자체 앱 분기(브라우저로 실행할래 특정 앱으로 실행할래를 묻는 팝업)는 사실 도메인만 올바르면 알아서 되는게 정상이다.
하지만 종종 이 기능이 무력화되는 경우가 있어서 테스트 해봤더니, 카카오톡 등의 웹뷰(=브라우저)를 내장한 앱에서는 소용이 없었음.
