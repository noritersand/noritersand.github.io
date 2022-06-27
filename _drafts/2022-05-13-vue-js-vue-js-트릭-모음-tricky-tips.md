---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[Vue.js] Vue.js 트릭 모음'
categories:
  - vuejs
tags:
  - javascript
  - vuejs
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)

#### 버전 정보

- Vue 3

## 개요

Vue 쓰면서 발견한 트릭들 모아봄.

## data 옵션 초기화 하기

프로퍼티가 많아지면 일일이 재할당하기 귀찮다.

```js
function initialState() {
  return {  
    message: '',
    numeric: 0,
    flag: false
  }
}
```

```js
data() {
  return initialState();
},
methods: {
  resetState() {
    // 이렇게 하지 말고
    // this.message = '';
    Object.assign(this.$data, initialState());
  }
}
```
