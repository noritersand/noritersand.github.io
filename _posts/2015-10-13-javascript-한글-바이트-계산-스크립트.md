---
layout: post
date: 2015-10-13 22:10:00 +0900
title: 'JavaScript: 한글 바이트 계산 스크립트'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - korean
  - byte
  - character
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

#### 코드 출처

- [http://programmingsummaries.tistory.com/239](http://programmingsummaries.tistory.com/239)

#### 유니코드를 3바이트로 계산

```js
function getByteLength(s,b,i,c){
    for (b = i = 0; c = s.charCodeAt(i++); b += c >> 11 ? 3 : c >> 7 ? 2 : 1);
    return b;
}

getByteLength('한 a'); // '한'=3, ' '=1, 'a'=1
```

#### 유니코드를 2바이트로 계산

```js
function getByteLength(s,b,i,c){
    for (b = i = 0; c = s.charCodeAt(i++); b += c >> 11 ? 2 : 1);
    return b;
}

getByteLength('한 a'); // '한'=2, ' '=1, 'a'=1
```
