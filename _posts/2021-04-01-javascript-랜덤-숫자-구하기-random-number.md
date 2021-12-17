---
layout: post
date: 2021-04-01 06:22:03 +0900
title: '[JavaScript] 랜덤 숫자 구하기 random number'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - random
  - math
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

별 거 없다.

```js
function fn(){
  console.log(getRandomNumber(2, 1)); // 1-2 사이 랜덤
  console.log(getRandomNumber(10, 5)); // 5-15 사이 랜덤
  console.log(getRandomNumber(4, 0)); // 0-3 사이 랜덤
  console.log(getRandomNumber(4, 1)); // 1-4 사이 랜덤
}

/**
 * 무작위로 양의 정수 구하기
 *
 * @param {number} range 무작위 굴림의 범위
 * @param {number} minimum 굴림의 최소값
 * @returns number
 */
function getRandomNumber(range, minimum) {
  return Math.floor(Math.random() * range + minimum);
}
```

읽어 볼 만한 글:

- [Wikipedia: Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
- [배열 요소 무작위로 섞기](https://ko.javascript.info/task/shuffle)
