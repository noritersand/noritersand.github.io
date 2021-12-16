---
layout: post
date: 2016-03-04 10:13:36 +0900
title: '[JavaScript] 남은 시간 카운트다운 countdown'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - time
  - interval
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

코드 출처:

- [https://www.sitepoint.com/build-javascript-countdown-timer-no-dependencies/](https://www.sitepoint.com/build-javascript-countdown-timer-no-dependencies/)
- [\[MDN\] String.prototype.padStart()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)

다음날 00시까지 남은 시간을 초 단위로 구하는 스크립트:

```js
// String.prototype.padStart() 폴리필 스크립트(IE는 padStart()가 없음)
if (!String.prototype.padStart) {
  String.prototype.padStart = function padStart(targetLength,padString) {
      targetLength = targetLength>>0; //truncate if number or convert non-number to 0;
      padString = String((typeof padString !== 'undefined' ? padString : ' '));
      if (this.length > targetLength) {
          return String(this);
      }
      else {
          targetLength = targetLength-this.length;
          if (targetLength > padString.length) {
              padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
          }
          return padString.slice(0,targetLength) + String(this);
      }
  };
}

// ref: 기준 시간. 이 코드에선 다음날 00시
var ref = new Date();
ref.setHours(0);
ref.setMinutes(0);
ref.setSeconds(0);
ref.setMilliseconds(0);
ref.setDate(ref.getDate() + 1);
// var ref = new Date('2021-03-13T00:00:00+09:00');

setInterval(function() {
  const now = new Date().getTime();
  const negative = ref.getTime() < now;
  const total = ref.getTime() - now;
  const seconds = negative ? '00' : Math.floor( (total/1000) % 60 ).toString().padStart(2, '0');
  const minutes = negative ? '00' : Math.floor( (total/1000/60) % 60 ).toString().padStart(2, '0');
  const hours = negative ? '00' : Math.floor( (total/(1000*60*60)) % 24 ).toString().padStart(2, '0');
  const days = negative ? '0' : Math.floor( total/(1000*60*60*24) ).toString();
  // console.log({ total, days, hours, minutes, seconds });
  console.log(days + ' days, ' + hours + ':' + minutes + ':' + seconds);
}, 1000);

// 실행 결과:
// 1 days, 00:00:03
// 1 days, 00:00:02
// 1 days, 00:00:01
// 1 days, 00:00:00
// 0 days, 23:59:59
// 0 days, 23:59:58
// 0 days, 23:59:57
// 0 days, 23:59:56
```
