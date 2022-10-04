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


## 강제 리랜더링

리랜더링을 수동으로 하는 것은 좋지 않은 방법이지만 어쩔 수 없이 해야 하는 경우가 있음. 가령 다른 써드 파티에서 input의 DOM value만 수정해버린다던지...

```js
import {getCurrentInstance, defineComponent} from 'vue'
export default defineComponent({
  setup() {
    const instance = getCurrentInstance();
    instance?.proxy?.$forceUpdate();
  }
})
```

```js
import { getCurrentInstance } from 'vue'
const instance = getCurrentInstance();
instance?.proxy?.$forceUpdate();
```

```js
Vue.createApp({
  // ...
  mounted() {
    // ...
    this.$forceUpdate();
  }
});
```
