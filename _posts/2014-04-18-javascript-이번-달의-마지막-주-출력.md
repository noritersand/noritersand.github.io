---
layout: post
date: 2014-04-18 18:49:00 +0900
title: '[JavaScript] 이번 달의 마지막 주 출력'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - date
  - last-week
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


```js
Date.prototype.addDate = function(value) {
  this.setDate(this.getDate() + value);
  return this;
}

function fn_weekOfYear() {
  var system = new Date();
  var target = new Date();

  // 이번 달의 마지막 날 구하기
  while (true) {
    target.addDate(1);
    if ((system.getMonth() < target.getMonth())
         || (system.getFullYear() < target.getFullYear())) {
      target.setDate(target.getDate() -1 );
      break;
      // 현재 시각에서 하루씩 더해 현재보다 달 수가 크거나 년 수가 크면 하루전으로 되돌림.
    }
  }
  console.log(system.toLocaleString() + ' \(현재시각\)');
  console.log(target.toLocaleString() + ' \(마지막 날\)');


  // 마지막날과 같은 주의 일요일 구하기
  target.setDate(target.getDate() - target.getDay());
  console.log(target.toLocaleString() + ' \(그 주의 먼저오는 일요일\)');


  // 일요일부터 일주일 출력
  var weekArry = ['일', '월', '화', '수', '목', '금', '토'];
  for (var loop = 0; loop < 7; loop++) {
    console.log((target.getMonth() + 1) + '.' + target.getDate() + '/' + weekArry[loop]);
    target.addDate(1);
  }
}

fn_weekOfYear();
```

계산한 일요일 값이 필요할 땐:

```js
// 일요일부터 일주일 출력
var weekArry = ['일', '월', '화', '수', '목', '금', '토'];
var sunday = new Date(target);
for (var loop = 0; loop < 7; loop++) {
  target.addDate(loop);
  console.log((target.getMonth() + 1) + '.' + target.getDate() + '/' + weekArry[loop]);
  target = new Date(sunday);
}
```
