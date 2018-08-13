---
layout: post
date: 2018-03-09 17:53:21 +09:00
title: 'JavaScript: 정규식 모음'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - regex
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
