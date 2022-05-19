---
layout: post
date: 2022-04-12 15:01:00 +0900
title: '[Vue.js] Vue.js 기본'
categories:
  - vuejs
tags:
  - javascript
  - vuejs
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[Vue.js\] Introduction](https://vuejs.org/guide/introduction.html)
- [\[Vue.js\] API](https://vuejs.org/api/)

#### 버전 정보

- Vue 3

## 개요

Vue.js 사용 방법 정리 글.

## 설치

[Quick Start](https://vuejs.org/guide/quick-start.html)

### 빌드해서 쓰기

Node.js로 Vue 컴파일이 포함된 패키지를 설치해 번들링하는 방식을 말한다.

[NPM](https://www.npmjs.com/package/vue)으로 설치한다:

```bash
npm install vue@latest
```

Vue 앱 만들기:

```bash
npm exec create-vue
# 혹은
npm init vue@latest
```

Vue 패키지는 컴파일과 웹 서버를 포함한다.

### 빌드 툴 없이 쓰기

크게 두 가지 방법이 있는데 그냥 자바스크립트 라이브러리처럼 external로 가져오거나:

```html
<!-- <script src="https://unpkg.com/vue@3"></script> -->
<!-- 아래와 같음 -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<div id="app">{{ message }}</div>
<script>
  const { createApp } = Vue;

  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
```

ECMAScript 모듈(통칭 ESM) 방식으로 불러오는 방식이 있다:

```html
<div id="app">{{ message }}</div>
<script type="module">
  import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';

  createApp({
    data() {
      return {
        message: "Hello Vue! ✌️"
      };
    },
  }).mount("#app");
</script>
```

## 선언적 렌더링

TODO 선언적 렌더링이 뭐야

TODO 구버전임

```js
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요'
  }
});
```

위 스크립트는 아이디가 'app'인 요소에 데이터 `message`를 할당한다는 뜻이다.

렌더링 후 콘솔에서`app.message = '야';`를 실행하면, '안녕하세요'가 '야'로 변경된다.  

가이드에 따르면 이것은 데이터와 DOM이 연결되어 모든 것이 반응형이기 때문에 가능한 일이며, 이를 *선언적 렌더링*이라 한다.

## Template Syntax

데이터를 HTML 태그에 바인딩하는 표현식들이다.

TODO 설명 션찮

### Text Interpolation

텍스트를 단순 렌더링한다. 프로퍼티를 중괄호 두 개로 감싸면 되는데(double curly braces), 공식 도움말에선 이 표현식을 '수염'(Mustache syntax)이라고 부른다.

```html
<!-- 태그 밖에서 -->
{{ value }}

<!-- 태그 안에서 -->
<TAG_NAME v-bind:attribute="value"/>
```

### Raw HTML

HTML을 escape하지 않고 그대로 출력하도록 한다.

TODO

### 속성 바인딩 Attribute Bindings

`v-` 접두어가 붙는 사용자 속성은 *디렉티브*라고 한다. `v-bind`는 이름처럼 값을 할당하기만 하는 디렉티브임.

`v-bind`는 매우 자주 쓰이기 때문에 단축 표현(shorthand syntax)이 있다:

```html
<div :id="dynamicId"></div>
<!-- 아래와 같음 -->
<div v-bind:id="dynamicId"></div>
```

TODO

### 자바스크립트 표현식 JavaScript Expressions

TODO

### Directives

TODO

## 조건부 렌더링 Conditional Rendering

### v-if

조건식이 `false`이면 요소가 비활성화 되는데, 단순히 CSS로 감추는게 아니라 DOM 자체가 사라진다.

```html
<div id="app-3">
  <p v-if="seen">보일락말락</p>
</div>
```

TODO 구버전임

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

### `v-else`

TODO

### `v-else-if`

TODO

### v-if on `<template>`

<!-- <template> 태그를 코드 블록 없이 그냥 쓰면 밑으로 다 안보이게 되니 주의할 것. HTML 표준 기술임 -->

다른 태그들을 묶기만 하는 더미 태그가 필요할 땐 `<template>` 태그를 쓰면 된다:

```html
<template v-if="ok">
  <h3>Header 3</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

이렇게 하면 `<template>`은 제어 처리만 하고 화면 랜더링 대상에선 빠진다.

### v-show

`false`일 때 `display: none` 스타일을 추가한다.

TODO

### v-show와 v-if의 차이

`v-if`는 조건식이 `false`일 때 아예 랜더링을 하지 않는다. 반면 `v-show`는 조건식과 상관 없이 일단 랜더링을 모두 한다는게 다르다.

그리고 `v-show`는 `<template>` 태그를 랜더링해버려서 `v-if`와 다르게 더미 태그로 쓸 수 없음.

## 목록 그리기 List Rendering

### v-for

```html
<div id="app-4">
    <ol>
        <li v-for="i in values">{{i.index}}</li>
    </ol>
</div>
```

TODO 구버전임

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    values: [
      { index: '하나' },
      { index: '둘' },
      { index: '셋' }
    ]
  }
});
```

렌더링 후 `app4.values.push({index: '넷'})`를 입력하면 반복문의 요소로 추가되며, 화면에 즉시 반영된다.

## 이벤트 핸들링 Event Handling

`v-on` 디렉티브로 이벤트를 바인딩하고 vue 앱의 `methods` 항목에 핸들러를 추가한다.

```html
<div id="app-5">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">메시지 뒤집기</button>
</div>
```

버튼 태그를 클릭하면 아래의 `reverseMessage` 함수가 실행된다:

```js
createApp({
  data() {
    return {
      message: '안녕하세요! Vue.js!'
    }
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
}).mount("#app");
```

여기서 `this`는 `reverseMessage`의 소유자 `methods`가 아니라 `app5`다. 요것은 Vue.js의 특성임.

속성 바인딩처럼 매우 자주 쓰이기 때문에 단축 표현이 있다:

```html
<button @click="increment">{{ count }}</button>
<!-- 아래와 같음 -->
<button v-on:click="increment">{{ count }}</button>
```

## 폼 바인딩 Form Input Bindings

`v-model` 디렉티브를 사용하면 사용자가 입력한 값과 화면에 보이는 값, 그리고 앱의 데이터가 동기화된다.

TODO 구버전임

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: '안녕하세요 Vue!'
  }
});
```

이 예시의 경우, `input` 태그의 `value` 값이 변경되면 vue 앱의 `message` 데이터도 같이 변경된다.

## 컴포넌트 Components

TODO 구버전임

```html
<div id="app-7">
    <ol>
        <todo-item></todo-item>
    </ol>
</div>
```

```js
Vue.component('todo-item', {
  template: '<li>할일 항목 하나입니다.</li>'
})

var app = new Vue({
  el: '#app-7'
});
```

이렇게 하면 `<todo-item>`은 `<li>`로 렌더링 된다.

## 컴포넌트: Props

부모한테 받아오는 읽기 전용 값.

## State

`data()` 같은 거

TODO

## Template Refs

DOM 요소에 직접 접근할 때 사용함:

```html
<input ref="focusMe">
```

```js
mounted() {
  this.$refs.focusMe.focus()
}
```

## `<template>`의 용도

랜더링 관련 디렉티브(`v-if`, `v-for` 등)와 같이 사용한다. 그리고 컴포넌트의 템플릿을 옵션과 함께 컴파일할 때 사용하기도 하는데, 이건 템플릿 컴파일러가 포함된 Vue 빌드에서만 지원된다.

이 태그를 빌드 없는 환경에서 컴포넌트 정의에 사용하려면 [vue3-sfc-loader](https://github.com/FranckFreiburger/vue3-sfc-loader)를 같이 써야함.
