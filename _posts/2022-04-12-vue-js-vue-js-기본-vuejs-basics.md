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

{% raw %}

#### 참고 문서

- [Vue.js \| Introduction](https://vuejs.org/guide/introduction.html)
- [Vue.js \| API](https://vuejs.org/api/)
- [https://v3-docs.vuejs-korea.org/](https://v3-docs.vuejs-korea.org/)

#### 테스트 환경 정보

- Vue 3


## 개요

Vue.js 사용 방법 정리 글.


## 설치

[Quick Start](https://vuejs.org/guide/quick-start.html)

### HTML에서 외부 스크립트로 사용하기

Vue를 자바스크립트 라이브러리처럼 사용하는 방법이다. 이쪽은 스크립트 태그 혹은 CDN 방식이라 한다.

External link:

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

ESM(ECMAScript Modules):

```html
<div id="app">{{message}}</div>
<script type="module">
import {createApp} from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';

createApp({
  ...
}).mount('#app');
</script>
```

### Node.js 모듈로 사용하기

Node.js CLI 환경에서 Vue 컴파일러로 빌드하는 방식을 말한다(Vue 패키지는 컴파일러와 웹 서버를 포함한다). Vue CLI 혹은 SFC(Single-File Components) 방식이라고 한다.

[npm](https://www.npmjs.com/package/vue)으로 설치한다:

```bash
npm install vue@latest
```

Vue 앱 만들기:

```bash
npm exec create-vue
# 혹은
npm init vue@latest
```

생성된 디렉터리로 이동해서:

```bash
npm install
npm run dev
```

### API 스타일

Vue 3부터는 Composition API 스타일과 Options API 스타일 중에 하나를 선택해야 한다.

두 스타일 모두 빌드 단계 없이 라이브러리 방식으로 사용할 수 있다. 라이브러리 방식인 경우 두 스타일의 코드 차이는 아래와 같다:

Options API 스타일:

```html
<div id="app">
  <h1>{{pageTitle}}</h1>
  <p>{{message}}</p>
</div>

<script type="module">
import { createApp } from '/lib/vue/vue.esm-browser.js';

createApp({
  data() {
    return {
      pageTitle: 'Vue 시작하기',
      message: 'Hello Vue! ✌️'
    };
  },
  created() {
    document.title += `: ${this.pageTitle}`;
  }
}).mount('#app');
</script>
```

Composition API 스타일:

```html
<div id="app">
  <h1>{{pageTitle}}</h1>
  <p>{{message}}</p>
</div>

<script type="module">
import { createApp, ref } from '/lib/vue/vue.esm-browser.js';

createApp({
  setup() {
    const pageTitle = ref('Vue 시작하기');
    const message = ref('Hello Vue! ✌️');

    document.title += `: ${pageTitle.value}`; // onCreated()는 없음

    return {
      pageTitle,
      message
    };
  }
}).mount('#app');
</script>
```

사용 가능한 메서드 등의 차이가 있긴 하지만, 어느 한 가지 스타일을 선택한다고 해서 다른 스타일을 못쓰는 것은 아니다. 나중에 얼마든지 변경할 수 있으니, 대충 입맛에 맞는 모양을 고르면 된다.

공식 가이드에 따르면, Options API는 Composition API 위에 구현되어 있으며, Options API 스타일이 좀 더 OOP 개발자들에게 익숙한 구조라고 한다. Composition API는 초보자에게 다소 어려울 수 있지만, 좀 더 유연하며 높은 복잡성을 처리하기 위해 설계되어 규모 있는 앱 구축에 적합하다고 한다.

더 자세한 내용은 [여기](https://vuejs.org/guide/introduction.html#api-styles)를 볼 것.

#### `<script setup>`

Composition API 스타일은 `setup()`과 `<script setup>`로 나뉜다. 이 중 `<script setup>`은 SFC로 빌드할 때만 사용할 수 있다. 이렇게 생겼다:

```html
<script setup>
import { ref, onMounted } from 'vue'

const el = ref()

onMounted(() => {
  el.value // <div>
})
</script>

<template>
  <div ref="el"></div>
</template>
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

**TODO** 설명 추가


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

**TODO** Composition API에선 두 가지로 나뉜다. 정리할 것

### Computed Properties

**TODO**


## Template Syntax

데이터를 HTML 태그에 바인딩하는 표현식들이다.

**TODO** 설명 션찮

### Text Interpolation\*

텍스트를 단순 렌더링한다. 프로퍼티를 중괄호 두 개로 감싸면 되는데(double curly braces), 공식 도움말에선 이 표현식을 '콧수염'(Mustache syntax)이라고 부른다.

```html
<p>{{mustache}}</p>
```

\* 수학의 보간법과는 관련이 없고, interpolate의 뜻 중 하나인 '덧붙이다' 혹은 '삽입하다'(insert)라는 뜻임 (출처: 뇌피셜)

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

**TODO**

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

**TODO**

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

### 클래스와 스타일 바인딩

#### 클래스

Object로 지정하기:

```
:class="{ 적용할클래스이름: 조건식 }"
```

```html
<div :class="{ active: isActive }"></div>
```

Object 배열로 지정하기:

```js
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```

```html
<div :class="[activeClass, errorClass]"></div> <!-- 'activeClass', 'errorClass'가 클래스로 추가됨 -->
```

#### 스타일

Object 방식:

```html
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

배열로 스타일 바인딩:

```js
data() {
  return {
    baseStyles: 'color: black;',
    overridingStyles: 'color: blue; font-weight: bold'
  }
}
```

```html
<div :style="[baseStyles, overridingStyles]"></div>
```


**TODO**


## 조건부 렌더링 Conditional Rendering

### v-if

조건식이 `false`이면 엘리먼트가 비활성화 되는데, 단순히 CSS로 감추는게 아니라 DOM 자체가 사라진다.

```js
export default {
  data() {
    return {
      seen: true
    };
  }
}
```

```html
<div>
  <p v-if="seen">보일락말락</p>
</div>
```

### v-else, v-else-if

else 블록 혹은 else-if 블록을 표현하는 디렉티브. 반드시 `v-if` 바로 다음에 오는 형제 엘리먼트여야 한다.

```html
<div>
  <p v-if="seen">보일락말락1</p>
  <p v-else-if="seen">보일락말락2</p>
  <p v-else="seen">보일락말락2</p>
</div>
```

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

이렇게 하면 `<template>`은 제어 처리만 하고 사라진다.

### v-show

`false`일 때 `display: none` 스타일을 추가한다.

**TODO**

### v-show와 v-if의 차이

`v-if`는 조건식이 `false`일 때 아예 렌더링을 하지 않는다. 반면 `v-show`는 조건식과 상관 없이 일단 렌더링을 모두 한다는게 다르다.

그리고 `v-show`는 `<template>` 태그를 렌더링해버려서 `v-if`와 다르게 더미 태그로 쓸 수 없음.


## 목록 그리기 List Rendering

### v-for

```js
export default {
  data() {
    return {
      values: [
        {index: '하나'},
        {index: '둘'},
        {index: '셋'}
      ]
    };
  }
}
```

```html
<div>
    <ul>
        <li v-for="i in values">{{i.index}}</li>
    </ul>
</div>
```

렌더링 후 `app4.values.push({index: '넷'})`를 입력하면 반복문의 요소로 추가되며, 화면에 즉시 반영된다.

**TODO**

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

`key`는 special attribute 중 하나로 Vue의 가상 DOM 알고리즘에서 vnodes(Vue's Virtual DOM)를 식별하기 위한 힌트로 사용된다. Vue는 키가 없을 때 가능한 한 원래의 자리에서 이동하지 않고 패치(patch)/재사용하려고 시도한다. 반면 키가 제공되면 키의 변경에 따라 요소를 재정렬하거나 제거/삭제한다. 이런 특성을 이용해 파일 첨부용 `<input type="file">` 엘리먼트를 키값을 바꿔 초기화하는 방법이 있다. [페이지 링크](/vuejs/vue-js-vue-js-트릭-모음-tricky-tips/#heading-file-input-초기화-하기)

`v-for`에서도 마찬가지인데, 키가 없을 때의 알고리즘 때문에 의도대로 작동하지 않을 수 있다. 따라서 요소를 재정렬 하거나 삭제를 해야 한다면 키를 제공할 것.

관련 글: [#1](https://vuejs.org/style-guide/rules-essential.html#use-keyed-v-for), [#2](https://vuejs.org/guide/essentials/list.html#maintaining-state-with-key)

```html
<select v-model="companyNo">
  <option v-for="(ele, index) in companyList" :key="index" :value="ele.companyNo">{{ele.companyName}}</option>
</select>

<!-- 혹은 -->

<select v-model="companyNo">
  <option v-for="ele in companyList" :key="ele.companyNo" :value="ele.companyNo">{{ele.companyName}}</option>
</select>
```

#### v-if 같이 쓰기

[가이드](https://vuejs.org/style-guide/rules-essential.html#avoid-v-if-with-v-for)에 따르면 같은 엘리먼트에 둘을 같이 쓰는 것은 좋지 않다고 하니 다음처럼 하위 엘리먼트로 분리하는 게 좋다:

```html
<ul>
  <template v-for="user in users">
    <li v-if="user.isActive">
      {{ user.name }}
    </li>
  </template>
</ul>
```


## 이벤트 핸들링 Event Handling

`v-on` 디렉티브로 이벤트를 바인딩하고 vue 앱의 `methods` 항목에 핸들러를 추가한다.

```html
<div>
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

이벤트 리스닝 표현식에는 modifier(수식어)가 제공되는데, 이벤트 전파를 막거나 이벤트의 기본 행동을 방지하는 등의 세부 설정을 할 수 있다:

- `.stop`: `Event.stopPropagation()`과 같음
- `.prevent`: `Event.preventDefault()`와 같음
- `.self`: 이벤트 발동 대상이 정확히 자기 자신일 때만 핸들러 실행
- `.capture`: 이 엘리먼트의 핸들러는 하위 엘리먼트의 핸들러보다 먼저 작동함
- `.once`: 한 번만 실행
- `.passive`: `Event.preventDefault()` 호출을 무시하고 이벤트의 기본 행동이 발생함

```html
<form @submit.prevent="onSubmit"></form>
<a @click.stop.prevent="doThat"></a>
```

key modifier와 mouse button modifier, system modifier도 있다. [https://vuejs.org/guide/essentials/event-handling.html#key-modifiers](https://vuejs.org/guide/essentials/event-handling.html#key-modifiers)

Key Modifiers:

- `.enter`
- `.tab`
- `.delete`: <kbd>delete</kbd>와 <kbd>backspace</kbd>에도 반응
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

Mouse Button Modifiers:

- `.left`
- `.right`
- `.middle`

System Modifier keys:

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

`.exact`도 있는데, 이 수식어는 system modifier 조합을 결정한다. 몇 가지 예를 들면:

- `@click.ctrl.exact`: <kbd>ctrl</kbd>키만 누르고 클릭했을 때 반응한다.
- `@click.ctrl`: <kbd>ctrl</kbd>외에 <kbd>shift</kbd>나 <kbd>alt</kbd>를 같이 누른 상태로 클릭해도 모두 반응한다.
- `@click.exact`: system modifier 키를 아무것도 누르지 않고 클릭했을 때 반응한다.

### $event

이벤트 핸들러에 전달하는 그 Event 인스턴스다. 뷰 표현식에서는 `$event`로 명시한다.

```html
<button type="button" @click="search($event)">push-me</button>
```

```js
export default {
  methods: {
    search(event) {
      console.log(event); // PointerEvent { ... }
    }
  }
}
```


## 폼 바인딩 Form Input Bindings

`v-model` 디렉티브를 사용하면 사용자가 입력한 값과 화면에 보이는 값, 그리고 앱의 데이터가 동기화된다.

```js
export default {
  data() {
    return {
      message: '안녕하세요 Vue!'
    };
  }
}
```

```html
<div>
  <p>{{message}}</p>
  <input v-model="message">
</div>
```

이 예시의 경우, `input` 태그의 `value` 값이 변경되면 vue 앱의 `message` 데이터도 같이 변경된다.

### 하나의 모델로 묶기

체크박스나 셀렉트박스(multiple 속성이 있을 때)는 하나의 `name` 속성으로 여러 값이 할당될 수 있는데, Vue에서는 `v-model`을 배열로 선언하여 적용할 수 있다:

```html
<select v-model="multipleSelected" multiple>
  <option value="A">A</option>
  <option value="B">B</option>
  <option value="C">C</option>
</select>
```

```js
export default {
  data() {
    return {
      multipleSelected: [] // ['A', 'B'] 이런식으로 할당됨
    };
  }
}
```

### true-value, false-value

체크박스에 한해서, 그리고 **여러 개를 하나의 모델로 묶지 않을 때에 한해** 체크/체크해제 각각의 값을 지정할 수 있다. 

```html
<input type="checkbox" v-model="yn" true-value="Y" :false-value="'N'" />
```

값이 문자열이면 콜론과 작은따옴표는 생략해도 된다.

### Modifiers

[https://vuejs.org/guide/essentials/forms.html#modifiers](https://vuejs.org/guide/essentials/forms.html#modifiers)

v-model에도 Event Modifiers와 같은 modifier가 제공된다.

- `.lazy`: 데이터 동기화 시점을 input 이벤트 후가 아니라 change 이벤트 후로 변경한다.
- `.number`: 사용자 입력을 자동으로 Number 타입으로 변환한다.
- `.trim`: 사용자 입력값 중 처음과 끝의 공백을 제거한다.


## Lifecycle

[Lifecycle Diagram](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

**TODO**


## 컴포넌트 Components

**TODO** 설명 추가

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

`<literal-template>`이라고 작성한 부분은 '사용자 정의 컴포넌트를 HTML 템플릿에 삽입'한다고 표현한다.

### Slots

컴포넌트를 부모 템플릿에 삽입할 때, 바디 부분에 작성한 값을 다루는 방법이다. [여기](https://vuejs.org/guide/components/slots.html)를 볼 것.

```xml
<!-- parent template -->
<FancyButton>
  Click me! <!-- slot content -->
</FancyButton>
```

```xml
<!-- child template -->
<button class="fancy-btn">
  <slot></slot> <!-- slot outlet -->
</button>
```

### Props

부모에게서 받아오는 읽기 전용 값. 부모에서 컴포넌트 표현식을 작성할 때 바인딩하는 값이 자식 컴포넌트의 `props`가 되는 식이다. **해당 값이 부모로부터 변경되면 자식 컴포넌트의 `props`도 변경된 값으로 갱신된다**.

참고: 부모가 넘긴 속성값은 자식 컴포넌트에서 `props`에서 선언하지 않아도 자동으로 상속된다. 이것은 `props`와 별개로 [fallthrough 속성](https://vuejs.org/guide/components/attrs.html#fallthrough-attributes)이라 부른다.

```xml
<some-picker :foo="'A3456'"><some-picker>
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

`required=true`일 때 초깃값이 null이면 경고 발생함.

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

들은 말로는 컴포넌트들의 계층 관계가 복잡할 수록 props를 활용하기 어려워지는데, 이 때 대신 쓰는게 [Vuex](https://vuex.vuejs.org/)라고...

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

`emits` 속성은 생략해도 작동하긴 하지만 있는게 권장되니 빼먹지 말자. 빼먹으면 [fallthrough 속성](https://vuejs.org/guide/components/attrs.html#fallthrough-attributes)이 된다.

### 컴포넌트와 v-model

ℹ️ 아래 내용은 Vue 3.4 버전에 추가된 `definedModel()`로 더 간단하게 구현할 수 있음. 단, Composition API 스타일만 가능

- [Component v-model \| Vue.js](https://vuejs.org/guide/components/v-model.html)
- [\<script setup\> \| Vue.js](https://vuejs.org/api/sfc-script-setup.html#definemodel)

---

`props`로 내려보내진 부모 컴포넌트의 반응형 상태값을 자식 컴포넌트에서 변경할 수 있게 하는 방법이다. 요약하면 emit을 이용한 `props`의 우회 변경인데, `props`는 읽기 전용이라서 직접 변경하는 것은 불가능하다. 따라서 여기서는 부모 컴포넌트에게 어떤 값으로 변경하라는 이벤트를 `emit()`하여 부모 컴포넌트가 스스로 변경하게 한다.

[가이드](https://vuejs.org/guide/components/v-model.html)를 보면 여러 구현 방법이 있지만, 이게 가장 좋음(하지만 코드 양도 많지):

```js
export const childComponent = {
  template: `
    <select v-model="computedModel">
      <option :value="null">널 값</option>
      <option :value="1">숫자 일</option>
    </select>
  `,
  props: ['selectedValue'],
  emits: ['update:selected-value'],
  computed: {
    computedModel: {
      get() {
        return this.selectedValue;
      },
      set(value) {
        this.$emit('update:selected-value', value);
      }
    }
  }
};
```

```html
<child-component v-model:selected-value="message"></child-component>
```

`selectedValue`는 부모 컴포넌트와 동기화할 `props`다. 이 변수의 setter가 필요한 상황이지만, 같은 이름의 `computed` 항목은 만들 수 없기 때문에 `computedModel`이라는 버퍼를 사용한다.

`emit()`으로 내보낼 이벤트 이름은 반드시 `update:`를 접두어로 사용해야 한다. 그리고 바인딩 표현식 `v-model:selected-value="message"`에서 `selected-value`는 props의 이름을 의미한다. 이 코드에선 부모 컴포넌트의 `message`가 `selectedValue`라는 이름의 props로 내려보내진다는 의미다. 

만약 모델 이름을 생략한 `v-model="message"`로 작성하면 `modelValue`라는 이름의 props로 내려간다. (`v-model:model-value="message"`라고 작성하면 고장나니까 주의) 그리고 이 때에는 이벤트 이름을 `update:selected-value`가 아니라 `update:model-value`라고 작성해야 한다. 

이것보다 간단한 게 있긴 한데:

```js
export const childComponent = {
  template: `
    <select :value="selectedValue" @input="$emit('update:selected-value', $event.target.value)">
      <option :value="null">널 값</option>
      <option :value="1">숫자 일</option>
    </select>
  `,
  props: ['selectedValue'],
  emits: ['update:selected-value']
};
```

이 방식은 모델 값이 `null`일 때 동기화가 제대로 안 되는 버그가 있다. 아마 내부에서 작동하는 유효성 검사기의 버그가 아닐까 싶음.


## Template Refs

DOM 엘리먼트 혹은 컴포넌트를 직접 다뤄야 때 사용한다.

선언은 `ref` 혹은 `:ref`로 하며:

```html
<input ref="focusMe">
```

이후 `$refs` 컴포넌트 인스턴스를 통해 접근할 수 있다.

```js
export default {
  mounted() {
    this.$refs.focusMe.focus()
  }
}
```

### v-for에서 사용하기

`ref`를 `v-for` 내부 엘리먼트에 사용하면, `ref` 값은 반복된 엘리먼트들을 갖는 배열이 된다:

```html
<template v-for="(ele, index) in elements" :key="index">
  <div ref="whoisdis" :style="{ backgroundColor: ele }" style="width: 100px; height: 100px"></div>
</template>
```

```js
export default {
  data() {
    return {
      elements: ['darkred', 'darkblue', 'darkgreen']
    };
  },
  mounted() {
    console.log(this.$refs.whoisdis); // Array(3) [ div, div, div ];
  }
};
```

그러나 공식 가이드에선 **원본 배열의 순서가 ref 배열의 순서와 다를 수 있기 때문에 배열 순서에 의존하는 코드는 작성하지 말라**고 권장한다. 그러니까 `v-for` 인덱스를 활용하는 방법은 잠재적 버그 가능성이 있다는 말이다.

만약 `v-for`로 그려넣는 어떤 입력값들이 있고, `v-model`을 통해 원시 DOM 객체에 접근하고 싶다면, `v-for` 반복 인덱스를 활용해 `ref`값의 이름을 지어주는 방법이 있다:

```html
<div v-for="(item, index) in items" :key="index">
  <input type="text" :id="'inpt' + ((index + 1) * 3)" :ref="'item' + index" v-model="item.value" />
</div>
```

```js
export default {
  data() {
    return {
      items: [
        {value: 'a'}, 
        {value: 'b'},
        {value: 'c'}
      ]
    };
  }
  mounted() {
    console.log(this.$refs.item0); // Array [ input#inpt3 ]
    console.log(this.$refs.item0[0]); // <input id="inpt3" type="text">
    console.log(this.$refs.item1[0]); // <input id="inpt6" type="text">
    console.log(this.$refs.item2[0]); // <input id="inpt9" type="text">
  },
};
```

다만 이렇게 했을 때 `ref`는 길이가 1인 배열이라는 점을 주의할 것.


## `<template>`의 용도

렌더링 관련 디렉티브(`v-if`, `v-for` 등)와 같이 사용한다. 그리고 컴포넌트의 템플릿을 옵션과 함께 컴파일할 때 사용하기도 하는데, 이건 템플릿 컴파일러가 포함된 Vue 빌드에서만 지원된다.

이 태그를 CDN 환경에서 컴포넌트 정의에 사용하려면 [vue3-sfc-loader](https://github.com/FranckFreiburger/vue3-sfc-loader)를 같이 써야함.


## 비동기 컴포넌트 제어: 특정 컴포넌트의 렌더링 멈추기

- [https://vuejs.org/guide/components/async.html](https://vuejs.org/guide/components/async.html)
- [https://vuejs.org/guide/built-ins/suspense.html#suspense](https://vuejs.org/guide/built-ins/suspense.html#suspense)
- [https://vueschool.io/articles/vuejs-tutorials/suspense-everything-you-need-to-know/](https://vueschool.io/articles/vuejs-tutorials/suspense-everything-you-need-to-know/)

**TODO** `<Suspense>`와 `async setup`을 이용해서 컴포넌트 렌더링 타이밍을 제어할 수 있다고 한다.


{% endraw %}
