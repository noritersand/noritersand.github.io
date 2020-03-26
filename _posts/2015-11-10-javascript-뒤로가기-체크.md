---
layout: post
date: 2015-10-13 22:10:00 +0900
title: '[JavaScript] 뒤로가기 체크'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - history.back
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```js
var fromHistoryBack = false;
var myHistory;
try {
  myHistory = JSON.parse(sessionStorage.getItem('myHistory'));
} catch (e) {};
if (myHistory) {
  if (myHistory[myHistory.length-1].href == window.location.href
      && myHistory[myHistory.length-1].referrer == document.referrer) {
    alert('새로고침 되었습니다.');
  } else {
    if (myHistory.length > 1) {
      if (myHistory[myHistory.length-2].href == window.location.href
          && myHistory[myHistory.length-2].referrer == document.referrer) {
        fromHistoryBack = true;
        myHistory.pop();
        sessionStorage.setItem('myHistory', JSON.stringify(myHistory));
      }
    }
    if (myHistory.length > 10 && !fromHistoryBack) {
      myHistory.shift();
      sessionStorage.setItem('myHistory', JSON.stringify(myHistory));
    }
    if (!fromHistoryBack) {
      myHistory.push({
        href: window.location.href,
        referrer: document.referrer
      });
      sessionStorage.setItem('myHistory', JSON.stringify(myHistory));
    }
  }
} else {
  var newHistory = [{
    href: window.location.href,
    referrer: document.referrer
  }];
  sessionStorage.setItem('myHistory', JSON.stringify(newHistory));
}
console.debug('fromHistoryBack:', fromHistoryBack);
```

history를 쿠키나 브라우저 스토리지에 별도로 축적하고 이를 비교해가며 뒤로가기 기능으로 랜딩된 페이지인지를 판단하는 소스. 단, 반드시 뒤로가기만 걸러내는것은 아니고 이전 페이지의 주소를 직접 호출하는 방식에도 반응하는 한계가 있다.
