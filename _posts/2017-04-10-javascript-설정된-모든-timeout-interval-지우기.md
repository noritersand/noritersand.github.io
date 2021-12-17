---
layout: post
date: 2017-04-10 16:53:00 +0900
title: '[JavaScript] 설정된 모든 timeout, interval 지우기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - timeout
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```js
var highestTimeoutId = setTimeout(";");
for (var i = 0 ; i < highestTimeoutId ; i++) {
  clearTimeout(i);
}
```

```js
var highestIntervalId = setInterval(";");
for (var i = 0 ; i < highestIntervalId ; i++) {
  clearInterval(i);
}
```

`setTimeout`이나 `setInterval`은 실행 후 숫자 타입의 아이디를 반환하는데 이 값은 함수 호출때마다 증가한다.

아이디가 계속 증가하는 숫자라는걸 이용해서, 가장 최근의 호출 반환값만큼 0부터 증가시켜 모두 지우는 방식의 코드.
