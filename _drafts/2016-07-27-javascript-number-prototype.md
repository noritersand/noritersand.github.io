---
layout: post
date: 2016-07-27 12:04:00 +0900
title: '[JavaScript] Number prototype'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - number
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)

Standard built-in Objects: Number


## 개요

`Number` 프로토타입의 필드/메서드 설명 글.


## 인스턴스 메서드 

### Number.prototype.toFixed()

```
toFixed([digits])
```

- `digits`: 소수점 아래 자릿수. `0`부터 `100`까지 지정할 수 있고 생략하면 `0`이다.

`Number` 인스턴스의 값을 지정한 소수점 아래 자릿수만큼의 길이를 갖는 문자열로 반환한다:

```js
(0.55).toFixed(20); // "0.55000000000000004441" 
```

만약 소수점 아래의 수가 지정한 길이보다 길면 반올림하여 반환한다:

```js
(0.1).toFixed(30); // "0.100000000000000005551115123126" 
(0.1).toFixed(18); // "0.100000000000000006" 
````

MDN에선 이를 고정 소수점 표기법(fixed-point notation)이라 설명한다.

**TODO: 부동 소수점으로 다뤄지고 있는 소수점 이하의 수를 고정 소수점으로 변환하여 표시하는 메서드... 라고 이해했으나 맞는지 검증 필요**
