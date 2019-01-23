---
layout: post
date: 2018-03-09 17:53:21 +0900
title: 'JavaScript: regex 정규식 모음'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - regex
  - 코드모음
---

### 숫자 - 통화 변환

```js
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

### 통화 - 숫자 변환 #1

```js
'1,000,000'.replace(/[^0-9]/g, '');
```

### 통화 - 숫자 변환 #2

```js
'123,123,123'.replace(/\,/g, '');
```

### 빈줄 제거(앞)

```js
var a = "\n\na\n\n";
a.replace(/^\s*[\r\n]/gm, "");
```
### 영문과 숫자가 아니면 공백으로 변경

```js
'q!1@2#3뀨?asd한글'.replace(/[^A-Za-z0-9]/gi, '뿅');
// 'q뿅1뿅2뿅3뿅뿅asd뿅뿅'
```
