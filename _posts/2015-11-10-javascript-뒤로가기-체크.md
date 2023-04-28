---
layout: post
date: 2015-10-13 22:10:00 +0900
title: '[JavaScript] 뒤로가기 체크'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - html-standard
  - history.back
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


## 개요

브라우저의 뒤로가기로 도달한 페이지인지 확인하는 방법 정리.


## BFCache

TODO


## 브라우저 storage를 활용한 커스텀 히스토리

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

페이지 이동 이력을 쿠키나 브라우저 스토리지에 별도로 축적하고 이를 비교해가며 뒤로가기 기능으로 랜딩된 페이지인지를 판단하는 소스. 단, 반드시 뒤로가기만 걸러내는것은 아니고 이전 페이지의 주소를 직접 호출하는 방식에도 반응하는 한계가 있다.


## 다른 방법

위 함수는 목록 더 보기 기능을 구현하기 위해 누군가 만들었던 것인데, 사실 더 간단하게 할 수도 있다.

가령 목록 페이지 A가 있고 상세 페이지 B가 있다 했을 때, 

1. A에서는 목록을 그리거나 더 보기 버튼을 누를 때 마다 페이지 번호와 목록 데이터를 세션스토리지에 저장한다.
2. B에서는 무조건 자기 자신의 location.pathname을 대충 'lastVisit'라는 이름으로 세션스토리지에 저장한다.
3. 다시 A로 왔을 때 'lastVisit'가 B인지 확인한다.
4. 3이 true면 세션스토리지에 저장된 목록 데이터와 페이지 번호를 재사용한다.
5. 동시에 A는 'lastVisit'를 지운다.

이렇게 하면 더 보기로 늘어난 목록 데이터를 상세 페이지에 다녀올 때마다 다시 처음부터 불러올 필요가 없어진다. 세션스토리지에 있는 걸 쓰면 되니까.

단, 상세 페이지를 갔다가 바로 목록으로 되돌아가는 게 아니라, 다른 어딘가에 들렸다가 목록으로 돌아왔을 때도 위의 3이 true로 판정될 것이다.

하지만 5 때문에 새로고침하면 완전히 새로 불러오니 이 정도는 괜찮음.

대충 코드는 이렇다:

목록 화면:

```js
let state = {
  bulletins: [],
  params: {
    page: 1,
    perPage: 5
  }
};

let lastVisit = sessionStorage.getItem('lastVisit');
sessionStorage.removeItem('lastVisit');
if (lastVisit === '/상세페이지주소') {
  try {
    state.bulletins = JSON.parse(sessionStorage.getItem('list'));
    state.params.page = JSON.parse(sessionStorage.getItem('page'));
    return;
  } catch (e) {
    // do nothing
  }
}
drawList({mode: 'replace'});

function drawList({mode = 'replace'} = {}) {
  let response = await axios.get('/목록조회');

  let list = mode === 'replace' ? response.data.list : state.bulletins.concat(response.data.list);
  state.bulletins = list;
  sessionStorage.setItem('list', JSON.stringify(list));

  let nextPage = response.data.nextPage;
  state.params.page = nextPage; // 다음 페이지
  sessionStorage.setItem('page', nextPage);
}
```

상세 화면: 

```js
sessionStorage.setItem('lastVisit', '/상세페이지주소');
```
