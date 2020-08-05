---
layout: post
date: 2020-03-17 15:01:00 +0900
title: '[Vue.js] Vue.js 기본'
categories:
  - vuejs
tags:
  - javascript
  - vuejs
  - basic
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [공식 사이트](https://vuejs.org/)
- [공식 가이드: 한글](https://kr.vuejs.org/v2/guide/index.html)
- [깃허브](https://github.com/vuejs/vue)

## 설치

```html
<!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

## 선언적 렌더링

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
가이드에 따르면 이것은 데이터와 DOM이 연결되어 모든 것이 반응형이기 때문에 가능한 일이며, 이를 **선언적 렌더링** 이라 한다.

## 표현식

vue 앱의 데이터를 바인딩하는 표현식. Vue 객체의 data 하위 항목들이 여기에 할당된다고 보면 됨.

```html
<!-- 태그 밖에서 -->
{{value}}

<!-- 태그 안에서 -->
<TAG_NAME v-bind:attribute="value"/>
```

`v-` 접두어가 붙는 사용자 속성은 **디렉티브** 라고 한다. `v-bind`는 이름처럼 값을 할당하기만 하는 디렉티브임.

## 제어문

### 조건문

`v-if` 디렉티브를 사용하며 이 값이 `false`이면 요소가 비활성화 되는데, 단순히 CSS로 감추는게 아니라 DOM에서 해당 요소가 삭제된다.

```html
<div id="app-3">
  <p v-if="seen">이제 나를 볼 수 있어요</p>
</div>
```

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

### 반복문

반복문은 `v-for` 디렉티브로 구현한다.

```html
<div id="app-4">
    <ol>
        <li v-for="i in values">{{i.index}}</li>
    </ol>
</div>
```

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

## 이벤트 핸들링

`v-on` 디렉티브로 이벤트를 바인딩하고 vue 앱의 `methods` 항목에 핸들러를 추가한다.

```html
<div id="app-5">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">메시지 뒤집기</button>
</div>
```

버튼 태그를 클릭하면 아래의 `reverseMessage` 함수가 실행된다:

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: '안녕하세요! Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
});
```

여기서 `this`는 `reverseMessage`의 소유자 `methods`가 아니라 `app5`다. 요것은 Vue.js의 특성임.

## 양방향 바인딩

`v-model` 디렉티브를 사용하면 입력 양식의 입력과 앱 상태가 양방향으로 바인딩된다.

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

## 컴포넌트

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
