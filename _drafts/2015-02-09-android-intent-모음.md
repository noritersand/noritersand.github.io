---
layout: post
date: 2015-02-09 13:24:00 +0900
title: 'Android: intent 모음 (비공개)'
categories:
  - android
tags:
  - android
  - intent
  - 코드모음
---

**비공개 글**

#### 관련 문서

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
