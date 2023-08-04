---
layout: post
date: 2022-05-13 00:00:00 +0900
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

{% raw %}

#### 참고한 문서

- [\[Vue.js\] Introduction](https://vuejs.org/guide/introduction.html)
- [\[Vue.js\] API](https://vuejs.org/api/)
- [https://v3-docs.vuejs-korea.org/](https://v3-docs.vuejs-korea.org/)

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


## 강제 리렌더링

리렌더링을 수동으로 하는 것은 좋지 않은 방법이지만 어쩔 수 없이 해야 하는 경우가 있음. 가령 다른 써드 파티에서 input의 DOM value만 쏙 수정해버린다던지...

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
import {getCurrentInstance} from 'vue'
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


## DOM 업데이트를 기다리는 방법

예를 들어 어떤 `<input>` 태그에 포커싱을 해야하는데, 아직 숨겨져 있거나 렌더링 전일 때는 코드 실행속도보다 렌더링이 느려서 `.focus()` 메서드가 제대로 작동하지 않는 경우가 있다.

이럴 때는 렌더링 후 콜백을 호출하게 하는 `nextTick()` 혹은 `$nextTick()`을 이용하면 된다:

```js
import { nextTick } from 'vue'

export default {
  methods: {
    doSomething() {
      nextTick(() => {
        this.$refs.someInputElement.focus();
      });
    }
  }
}
```

```js
export default {
  methods: {
    doSomething() {
      this.$nextTick(function() {
        this.$refs.someInputElement.focus();
      });
    }
  }
}
```

`nextTick()`과 `$nextTick()`의 차이는 다음과 같다:

> 전역 nextTick()과의 유일한 차이점은 this.$nextTick()에 전달된 콜백이 현재 컴포넌트 인스턴스에 바인딩된 this 컨텍스트를 갖는다는 것입니다.
>
> https://v3-docs.vuejs-korea.org/api/component-instance.html#nexttick

따라서 화살표 함수를 사용하지 않아도 컴포넌트 인스턴스를 콜백 함수 내에서 `this`로 공유한다.

콜백 함수 대신 async/await을 활용하면 아래처럼 된다:

```js
import { nextTick } from 'vue'

export default {
  methods: {
    async doSomething() {
      // DOM 업데이트 전
      await nextTick();
      // DOM 업데이트 후
      this.$refs.someInputElement.focus();
    }
  }
}
```

```js
export default {
  methods: {
    async doSomething() {
      // DOM 업데이트 전
      await this.$nextTick();
      // DOM 업데이트 후
      this.$refs.someInputElement.focus();
    }
  }
}
```


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


## Watch를 잠시 중단하거나 피하는 방법

아직 못찾음 ㅋ


## file input 초기화 하기

키 값만 바꿔주면 Vue가 렌더링을 다시 한다는 점을 이용하는 방법이 있다:

```html
<input type="file" :key="fileInputKey">
```

```js
export default {
  data() {
    return {
      fileInputKey: 0
    }
  },
  methods: {
    brandNewFileInput() {
      this.fileInputKey++;
    }
  }
}
```

이 외에 reactive state를 배열로 선언해서 초기화 대신 배열 pop-push 하는 방법이 있는데, 쪼오끔 귀찮.

{% endraw %}
