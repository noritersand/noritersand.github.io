---
layout: post
date: 2016-03-04 10:13:36 +0900
title: '[JavaScript] 남은 시간 카운트다운'
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

```html
<script>
  window.onload = function() {
    setInterval(function() {
      var target = document.getElementsByName("dt_now")[0];
      target.value = new Date();
    }, 1000);
  }
</script>

<input type="text" size="32" name="dt_now" value="00" />
```

이건 완전 신입 때 만든거고 😂

다음날 00시까지 남은 시간을 구하는 스크립트:

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
ref.setDate(ref.getDate() + 1);
ref.setHours(0);
ref.setMinutes(0);
ref.setSeconds(0);
ref.setMilliseconds(0);

setInterval(function() {
  const total = ref.getTime() - new Date().getTime();
  const seconds = Math.floor( (total/1000) % 60 );
  const minutes = Math.floor( (total/1000/60) % 60 );
  const hours = Math.floor( (total/(1000*60*60)) % 24 );
  // const days = Math.floor( total/(1000*60*60*24) );

  // return {
  //   total, days, hours, minutes, seconds
  // };

  console.log(hours.toString().padStart(2, '0')
      + ':' + minutes.toString().padStart(2, '0')
      + ':' + seconds.toString().padStart(2, '0'));
}, 1000);

// 실행 결과:
// 00:00:03
// 00:00:02
// 00:00:01
// 00:00:00
// 23:59:59
// 23:59:58
// 23:59:57
// 23:59:56
```
