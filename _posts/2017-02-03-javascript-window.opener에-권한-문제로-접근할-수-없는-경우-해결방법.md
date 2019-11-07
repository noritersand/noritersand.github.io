---
layout: post
date: 2017-02-03 13:39:10 +0900
title: '[JavaScript] window.opener에 권한 문제로 접근할 수 없는 경우 해결방법'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - window.open
  - opener
---

* Kramdown table of contents
{:toc .toc}

```js
opener.name;
"액세스가 거부되었습니다." // 익스
SecurityError: Blocked a frame with origin "http://www.daum.net" from accessing a cross-origin frame // 크롬
Error: Permission denied to access property 'name' // 파폭
```

window.open()으로 생성된 새 창(자식 창)에서 부모 창에 접근할 수 없는 경우가 있는데, 이것은 보통 부모 창과 자식 창의 호스트명이나 프로토콜, 포트가 다를때 [동일 출처 정책(same-origin policy)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)에 따라 브라우저가 접근을 차단하는 경우다.

이를 해결하는 방법은 두 가지가 있다.

## document.domain 변경

부모 창의 도메인은 'abc.tistory.com'이고 자식 창의 도메인은 'def.tistory.com'일 때 두 창의 도메인을 모두 'tistory.com'으로 변경한다. 도메인은 원하는 값으로 마음대로 변경할 수 있는건 아니고 서브도메인을 삭제하는것만 허용된다. 가령 실제 도메인이 'noritersand.tistory.com'일 때 변경 가능한 값은 'noritersand.tistory.com' 혹은 'tistory.com'이다.

```js
document.domain; // 'noritersand.tistory.com'
document.domain = 'aa.tistory.com'; // Error: Illegal document.domain value
document.domain = 'tistory.com'; // 'tistory.com'
document.domain = 'daum.net'; // Error: Illegal document.domain value
```

## 부모 창과 같은 도메인으로 이동

먼저 open 함수를 사용하여 다른 도메인의 자식 창을 생성한다.

![](/images/window-opener-1.png)

앞서 말했다시피 도메인이 다르기 때문에 자식 창에선 부모 창에 접근할 수 없다. 따라서 자식 창의 페이지를 부모와 같은 도메인의 페이지로 변경한다. (SUBMIT 혹은 location.href)

![](/images/window-opener-2.png)

이제 도메인이 같아졌으므로 opener를 통해 부모 창 객체에 접근할 수 있다.

![](/images/window-opener-3.png)
