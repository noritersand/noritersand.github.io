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
- [https://v3-docs.vuejs-korea.org/](https://v3-docs.vuejs-korea.org/)

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
<div id="app">{{message}}</div>
<script>
const {createApp} = Vue;

createApp({
  ...
}).mount('#app')
</script>
```

ECMAScript 모듈(통칭 ESM) 방식으로 불러오는 방식이 있다:

```html
<div id="app">{{message}}</div>
<script type="module">
import {createApp} from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';

createApp({
  ...
}).mount('#app');
</script>
```


## 선언적 렌더링이란?

```html
<p>{{message}}</p>

<script>
createApp({
  data() {
    return {
      message: '안녕하세요'
    };
  },
}).mount('#app');
</script>
```

위 예시는 반응형 상태값(reactive state)인 `message`를 선언하고 `<p>` 태그 바디에 바인딩하는 코드다. `message`의 값이 변경되면 화면에서도 즉시 바뀐 값이 적용된다.

가이드에 따르면 이것은 데이터와 DOM이 연결되어 모든 것이 반응형이기 때문에 가능한 일이며, 이를 *선언적 렌더링*이라 한다고...


## JSX

Vue도 JSX 사용 가능함:

```html
const vnode = <div>hello</div>
```

[참고](https://vuejs.org/guide/extras/render-function.html#jsx-tsx)

[@vue/babel-plugin-jsx](https://github.com/vuejs/babel-plugin-jsx) 있으면 된다고 한다.

**TODO 설명 추가**


## Reactive State

컴포넌트의 반응형 상태값(혹은 프로퍼티)을 말한다. 

Options API에선 `data()` 메서드로 정의한다:

```js
createApp({
  data() {
    return {
      docs: [
        {title: "어디어디", url: "어디어디"},
        {title: "어디어디", url: "어디어디"},
      ],
      message: "Hello Vue! ✌️",
      sequence: 0
    };
  },
  ...
});
```

TODO Composition API에선 두 가지로 나뉘는데, `setup()`에서 객체를 반환하는 방식과, `ref`로 할당하는 방식이 있다. 아마도 🤭

### Computed Properties

TODO


## Template Syntax

데이터를 HTML 태그에 바인딩하는 표현식들이다.

TODO 설명 션찮

### Text Interpolation

텍스트를 단순 렌더링한다. 프로퍼티를 중괄호 두 개로 감싸면 되는데(double curly braces), 공식 도움말에선 이 표현식을 '콧수염'(Mustache syntax)이라고 부른다.

```html
<p>{{mustache}}</p>
```

### Raw HTML

HTML을 escape하지 않고 그대로 출력한다.

```html
<p v-html="rawHtml"></p>
```

XSS 취약점이 발생할 수 있으니 주의할 것.

#### mustache와 v-html 비교

이런 데이터가 있을 때:

```js
data() {
  return {
    rawHtml: '<br>',
    entities: '&lt;br&gt;'
  }
}
```

둘의 차이는 다음과 같다:

- `rawHtml`: mustache는 화면에 `<br>`이 그대로 보인다. v-html은 문서에 `<br>` 태그를 삽입한다.
- `entities`: mustache는 화면에 `&lt;br&gt;`이 그대로 보인다. v-html은 화면에 `<br>`이 그대로 보인다.

### 속성 바인딩 Attribute Bindings

[https://vuejs.org/api/built-in-directives.html#v-bind](https://vuejs.org/api/built-in-directives.html#v-bind)

`v-` 접두어가 붙는 사용자 속성은 *디렉티브*라고 하는데, 속성 바인딩에는 `v-bind` 디렉티브를 사용한다.

`v-bind`는 매우 자주 쓰이기 때문에 단축 표현(shorthand syntax)이 있다:

```html
<div :id="dynamicId"></div>
<!-- 아래와 같음 -->
<div v-bind:id="dynamicId"></div>
```

TODO

### Built-in Directives

현재(2023-03-06) 빌트인 디렉티브는 이마안큼 있다.

- v-text: 콧수염, 텍스트 써넣기
- v-html: HTML 그대로 삽입
- v-show:
- v-if:
- v-else:
- v-else-if:
- v-for:
- v-on: 이벤트 리스너 등록 (shorthand: `@`)
- v-bind: 속성 바인딩 (shorthand: `:`, `.prop`이나 `.attr` modifier는 `.` 함께 사용)
- v-model: 
- v-slot (shorthand: `#`)
- v-pre: 
- v-once: 
- v-memo: 
- v-cloak: 

TODO

### 자바스크립트 표현식 JavaScript Expressions

콧수염(Mustache syntax)이나 Vue 디렉티브 속성의 내부에 작성한 코드는 자바스크립트 표현식으로 작동한다.

```html
<p v-bind:class="['a', 'b']">{{1 + 2}}<p>
```

예를 들어 위 코드의 결과는 아래와 같다:

```html
<p class="a b">3</p>
```

모든 자바스크립트 코드가 허용되는 것은 아니다. 도움말에 따르면 하나의 단일 표현식(one single expression)만 가능하며, 값으로 평가되는 표현식이어야 한다고 한다. 메서드 호출은 가능하지만, 선언식이나 반환식은 허용되지 않는다. 그리고 블록`{}`도 사용할 수 없으며 구문(statements)도 불가능.

#### 전역 변수의 접근 제한

자바스크립트 표현식에서 접근 가능한 전역 참조는 정해진 것 외에는 불가능하다. 예를 들어 사용자 정의 전역 변수와 window는 표현식에서 쓸 수 없다. 대신 메서드를 통한 간접 참조는 가능함(표현식에선 메서드를 호출하고. 메서드에서 전역 변수에 접근).

만약 표현식에서 직접 참조해야 하는 전역 변수가 있다면 아래처럼 하면 됨:

```js
let someGlobalVariable = 'foo';

const app = createApp({
  // 생략
});

app.config.globalProperties.someGlobalVariable = someGlobalVariable;

const vm = app.mount("#app");
```

`app.config`에 대한 설명은 [여기](https://vuejs.org/api/application.html#app-config)를 참고할 것.


## 조건부 렌더링 Conditional Rendering

### v-if

조건식이 `false`이면 요소가 비활성화 되는데, 단순히 CSS로 감추는게 아니라 DOM 자체가 사라진다.

```js
export default {
  data: {
    seen: true
  }
}
```

```html
<div id="app-3">
  <p v-if="seen">보일락말락</p>
</div>
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

이렇게 하면 `<template>`은 제어 처리만 하고 화면 렌더링 대상에선 빠진다.

### v-show

`false`일 때 `display: none` 스타일을 추가한다.

TODO

### v-show와 v-if의 차이

`v-if`는 조건식이 `false`일 때 아예 렌더링을 하지 않는다. 반면 `v-show`는 조건식과 상관 없이 일단 렌더링을 모두 한다는게 다르다.

그리고 `v-show`는 `<template>` 태그를 렌더링해버려서 `v-if`와 다르게 더미 태그로 쓸 수 없음.


## 목록 그리기 List Rendering

### v-for

```js
export default {
  data: {
    values: [
      {index: '하나'},
      {index: '둘'},
      {index: '셋'}
    ]
  }
};
```

```html
<div id="app-4">
    <ul>
        <li v-for="i in values">{{i.index}}</li>
    </ul>
</div>
```

렌더링 후 `app4.values.push({index: '넷'})`를 입력하면 반복문의 요소로 추가되며, 화면에 즉시 반영된다.

TODO:

- 단순 배열 반복
- 객체 배열 반복
- 다중 배열
- for-of
- 배열 아니고 그냥 객체
- 범위를 하드코딩으로 지정하기
- v-for와 v-if
- 같은 요소에서 index나 element에 접근
- v-for와 컴포넌트
- 자바스크립트 템플릿 리터럴 사용 시 주의할 점

#### key

가이드에 따르면 `key` 속성을 항상 명시하는 게 좋다고 한다. 관련 글: [#1](https://vuejs.org/style-guide/rules-essential.html#use-keyed-v-for), [#2](https://vuejs.org/guide/essentials/list.html#maintaining-state-with-key)

방법은 대충 아래와 같다:

```html
<select v-model="companyNo">
  <option v-for="(ele, index) in companyList" :key="index" :value="ele.companyNo">{{ele.companyName}}</option>
</select>

<!-- 혹은 -->

<select v-model="companyNo">
  <option v-for="ele in companyList" :key="ele.companyNo" :value="ele.companyNo">{{ele.companyName}}</option>
</select>
```


## 이벤트 핸들링 Event Handling

`v-on` 디렉티브로 이벤트를 바인딩하고 vue 앱의 `methods` 항목에 핸들러를 추가한다.

```html
<div id="app-5">
    <p>{{message}}</p>
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
<button @click="increment">{{count}}</button>
<!-- 아래와 같음 -->
<button v-on:click="increment">{{count}}</button>
```

### Event Modifiers

이벤트 리스닝 표현식에는 modifier가 제공되는데, 이벤트 전파를 막거나 이벤트의 기본 행동을 방지하는 등의 세부 설정을 할 수 있다:

- `.stop`: `Event.stopPropagation()`과 같음
- `.prevent`: `Event.preventDefault()`와 같음
- `.self`: 이벤트 발동 대상이 정확히 자기 자신일 때만 핸들러 실행
- `.capture`: 이 요소의 핸들러는 하위 요소의 핸들러보다 먼저 작동함
- `.once`: 한 번만 실행
- `.passive`: `Event.preventDefault()` 호출을 무시하고 이벤트의 기본 행동이 발생함

```html
<form @submit.prevent="onSubmit"></form>
<a @click.stop.prevent="doThat"></a>
```

이 외에 key modifiers와 mouse button modifiers도 있다: [https://vuejs.org/guide/essentials/event-handling.html#key-modifiers](https://vuejs.org/guide/essentials/event-handling.html#key-modifiers)

### $event

이벤트 핸들러에 전달하는 그 Event 인스턴스다. 뷰 표현식에서는 `$event`로 명시한다.

```html
<button type="button" @click="search($event)">push-me</button>
```

```js
methods: {
  search(event) {
    console.log(event); // PointerEvent { ... }
  }
}
```


## 폼 바인딩 Form Input Bindings

`v-model` 디렉티브를 사용하면 사용자가 입력한 값과 화면에 보이는 값, 그리고 앱의 데이터가 동기화된다.


```js
export default {
    data: {
    message: '안녕하세요 Vue!'
  }
};
```

```html
<div id="app-6">
  <p>{{message}}</p>
  <input v-model="message">
</div>
```

이 예시의 경우, `input` 태그의 `value` 값이 변경되면 vue 앱의 `message` 데이터도 같이 변경된다.


## Lifecycle

[Lifecycle Diagram](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

TODO


## 컴포넌트 Components

TODO 설명 추가

```js
// literal-template.js
export const literalTemplate = {
  template: `
    <p>응애 나 아기 컴포넌트</p>
    <div><button type="button" @click="probe">probe</button></div>
    <p>{{message}}</p>
  `,
  data() {
    return {
      message: 'This is literal template message'
    }
  }
};
```

```js
// parent.js
import {literalTemplate} from '/literal-template.js'

createApp({
  components: {
    literalTemplate
  }
}).mount("#app");
```

```html
<literal-template></literal-template>
```

### Props

부모에게서 받아오는 읽기 전용 값. 부모에서 컴포넌트 표현식을 작성할 때 바인딩하는 값이 자식 컴포넌트의 props가 되는 식이다.
아래처럼 넘김:

```xml
<some-picker :pick-this="'A3456'"><some-picker>
```

받는 방법은 API 방식에 따라 다르다.

Options API 문자열 배열 방식:

```js
export default {
  props: ['foo'],
  created() {
    console.log(this.foo);
  }
}
```

Options API 객체 선언 방식:

```js
// 기본 값이나 데이터 타입, 필수 여부를 지정할 수 있음
export default {
  props: {
    foo: {
      type: [String, Number], // String과 Number 둘 다 허용
      default: null,
      required: false
    }
  },
  created() {
    console.log(this.foo);
  }
}
```

`required=true`일 때 초기값이 null이면 경고 발생함.

Composition API `<script setup>` 스타일에서 문자열 배열 방식:

```html
<script setup>
const props = defineProps(['foo'])

console.log(props.foo)
</script>
```

Composition API `<script setup>` 스타일에서 객체 선언 방식:

```html
<script setup>
const props = defineProps({
  statusText: 'String'
  required: true, // 필수 프로퍼티로 지정
  validator(value) {
    return ['success', 'fail'].includes(value); // 'success' 혹은 'fail'만 허용
  }
});

console.log(props.statusText)
</script>
```

Composition API `setup()` 스타일:

```js
export default {
  props: ['foo'],
  setup(props) {
    // setup()은 첫 번째 인자로 props를 받습니다.
    console.log(props.foo)
  }
}
```

들은 말로는 컴포넌트들의 계층 관계가 복잡할 수록 props를 활용하기 어려워져서 잘 안쓰게 된다 함. 그래서 대신 쓰는게 [Vuex](https://vuex.vuejs.org/)라고...

### Event emitting

emit은 상위 컴포넌트로 이벤트를 내보내는 것을 말한다.

```
$emit(eventType, ...args)
```

`$emit()` 함수를 호출해서 구현한다. 이 함수의 첫 번째 인자는 이벤트 타입, 두 번째부터는(...args) 전달할 메시지다. 이벤트 타입은 마음대로 정의하면 된다. 그리고 해당 이벤트 타입의 리스너를 부모 측에서 등록하는 식:

```js
export const emittingTest = {
  template: `
    <button type="button" @click="$emit('custom-event:press', '이 메시지는 컴포넌트에서 시작되어...')">누르면 발싸합니다</button>
  `,
  emits: ['custom-event:press']
};
```

```html
<emitting-test @custom-event:press="handle"></emitting-test>
```

`emits` 속성은 생략해도 작동하긴 하지만 있는게 권장되니 빼먹지 말자. 빼먹으면 경고 뜸. [fallthrough 속성](https://vuejs.org/guide/components/attrs.html)과 관련 있다고 함.

### 컴포넌트와 v-model

부모 컴포넌트의 반응형 모델과 자식 컴포넌트를 연결하는 방법이다.

[가이드](https://vuejs.org/guide/components/v-model.html)를 보면 여러가지 구현이 있지만, 이게 가장 좋음(하지만 코드 양도 많지):

```js
export const childComponent = {
  template: `
    <select v-model="computedModel">
      <option :value="null">널 값</option>
      <option :value="1">숫자 일</option>
    </select>
  `,
  props: ['selected'],
  emits: ['update:selected'],
  computed: {
    computedModel: {
      get() {
        return this.selected;
      },
      set(value) {
        this.$emit('update:selected', value);
      }
    }
  }
};
```

```html
<child-component v-model:selected="message"></child-component>
```

emit 이름은 `update:`를 반드시 포함해야 한다. 그리고 `v-model:selected="message"`에서 `:selected`는 사용자 지정값으로 자식 컴포넌트의 prop 이름으로 사용된다. 만약 생략하면 기본값으로 `modelValue`가 이름이 된다. 그렇다고 또 `modelValue`를 명시하면 고장나니까 주의.

이것보다 간단한 게 있긴 한데:

```js
export const childComponent = {
  template: `
    <select :value="selected" @input="$emit('update:selected', $event.target.value)">
      <option :value="null">널 값</option>
      <option :value="1">숫자 일</option>
    </select>
  `,
  props: ['selected'],
  emits: ['update:selected']
};
```

이 방식은 모델 값이 `null`일 때 동기화가 제대로 안되는 버그가 있다. 아마 내부에서 작동하는 유효성 검사기의 버그가 아닐까 싶음.


## Template Refs

DOM 요소 혹은 컴포넌트를 직접 다뤄야 때 사용한다.

선언은 `ref` 혹은 `:ref`로 하며:

```html
<input ref="focusMe">
```

이후 인스턴스의 `$refs` 객체를 통해 접근할 수 있다.

```js
export default {
  mounted() {
    this.$refs.focusMe.focus()
  }
}
```


## `<template>`의 용도

렌더링 관련 디렉티브(`v-if`, `v-for` 등)와 같이 사용한다. 그리고 컴포넌트의 템플릿을 옵션과 함께 컴파일할 때 사용하기도 하는데, 이건 템플릿 컴파일러가 포함된 Vue 빌드에서만 지원된다.

이 태그를 빌드 없는 환경에서 컴포넌트 정의에 사용하려면 [vue3-sfc-loader](https://github.com/FranckFreiburger/vue3-sfc-loader)를 같이 써야함.


## $attrs

TODO


## 페이지 로딩 중 표현식 감추기

[https://vuejs.org/api/built-in-directives.html#v-cloak](https://vuejs.org/api/built-in-directives.html#v-cloak)

빌드를 하지 않는 뷰 환경에서 쓸만한 방법이다.

렌더링이 완료되기 전에는 콧수염을 포함한 뷰 표현식들이 그대로 보일 수 있는데 이 때 `v-clock`을 활용한다.

`v-clock`은 연관된 컴포넌트의 마운트가 완료되면 사라지는 속성이다. 이를 이용해서 `v-clock`이 있는 요소는 화면에서 감춰버리는 것:

```html
<style>
[v-cloak] {
  display: none;
}
</style>

<div v-cloak>
  {{message}}
</div>
```
