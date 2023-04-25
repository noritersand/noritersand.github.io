---
layout: post
date: 2021-04-01 06:22:03 +0900
title: '[JavaScript] 랜덤 숫자 구하기'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - random
  - math
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```js
/**
 * 무작위로 양의 정수 구하기
 *
 * @param {number} range 무작위 굴림의 범위
 * @param {number} minimum 굴림의 최소값
 * @returns number
 */
function getRandomInt(range, minimum) {
  return Math.floor(Math.random() * range + minimum);
}

  console.log(getRandomInt(2, 1)); // 1-2 사이 랜덤
  console.log(getRandomInt(10, 5)); // 5-15 사이 랜덤
  console.log(getRandomInt(4, 0)); // 0-3 사이 랜덤
  console.log(getRandomInt(4, 1)); // 1-4 사이 랜덤
```

읽어 볼 만한 글:

- [\[MDN\] Math.random](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)
- [Wikipedia: Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
- [배열 요소 무작위로 섞기](https://ko.javascript.info/task/shuffle)
