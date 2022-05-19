---
layout: post
date: 2022-05-19 22:42:38 +0900
title: '[Vue.js] Vue.js: Options API'
categories:
  - vuejs
tags:
  - javascript
  - vuejs
  - options-api
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[Vue.js\] Introduction](https://vuejs.org/guide/introduction.html)
- [\[Vue.js\] API](https://vuejs.org/api/)

#### 버전 정보

- Vue 3

## 개요

Vue는 Options API 혹은 Composition API 스타일로 작성할 수 있는데, 이 중 Options API에 해당하는 것들만 정리함.

## Options API

[\[Vue.js\] Options: State](https://vuejs.org/api/options-state.html)

### data()

state를 선언할 때 사용한다:

```js
data() {
  return {
    message: 'Hello World!',
    counter: { count: 0 }
  }
}
```

`data()`에서 반환한 프로퍼티는 ... TODO

### props

```xml
<some-picker :pick-this="'A3456'" :pick-these="['A1234', 'B5678', 'B6789']"><some-picker>
```

```js
props: {
  pickThis: {
    type: String,
    default: null,
    required: false
  },
  pickthese: {
    type: Array(String),
    default: [],
    required: false
  }
}
```

### created()

> After init Options API

데이터와 이벤트가 활성화된 시점. `mounted()`보다 이르다.

TODO

### mounted()

> After initial render, create & insert DOM nodes

DOM이 생성된 시점. `created()`보다 나중이다.

TODO

### methods()

```js
createApp({
  ...
  methods: {
    sayHello() {
      console.log('Hello!');
    }
  }
}).mount('#app')
```

TODO

### computed()

TODO

### watch()

```js
createApp({
  ...
  watch: {
    todoId() {
      this.fetchData()
    }
  }
}).mount('#app')
```
