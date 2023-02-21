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
- [\[Vue.js\] Options API](https://vuejs.org/api/#options-api)
- [\[Vue.js\] Options: State](https://vuejs.org/api/options-state.html)
- [https://v3-docs.vuejs-korea.org/](https://v3-docs.vuejs-korea.org/)

#### 버전 정보

- Vue 3


## 개요

Vue는 Options API 혹은 Composition API 스타일로 작성할 수 있는데, 이 중 Options API에 해당하는 것들만 정리함.


## Component Instance

- `$data`: 
- `$props`: 
- `$el`: 
- `$options`: 
- `$parent`: 
- `$root`: 
- `$slots`: 
- `$refs`: 
- `$attrs`: 
- `$watch()`: 
- `$emit()`: 
- `$forceUpdate()`: 
- `$nextTick()`: 


## data()

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


## props

우선 선언을 해야 하는데, 문자열 배열과 객체 선언 두 가지 방식이 있다.

```js
props: ['foo'],
created() {
  console.log(this.foo);
}
````

객체 선언 방식은 기본 값이나 데이터 타입, 필수 여부를 지정할 수 있음:

```js
props: {
  foo: {
    type: String,
    default: null,
    required: false
  }
}
```

그리고 요렇게 넘김:

```xml
<some-picker :pick-this="'A3456'" :pick-these="['A1234', 'B5678', 'B6789']"><some-picker>
```

사용할 때는 data와 똑같이 쓰면 된다. 읽기 전용 값이라 재할당은 불가능.


## created()

> After init Options API

데이터와 이벤트가 활성화된 시점. `mounted()`보다 이르다.

TODO


## mounted()

> After initial render, create & insert DOM nodes

DOM이 생성된 시점. `created()`보다 나중이다.

TODO


## methods()

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


## computed()

- [https://vuejs.org/api/options-state.html#computed](https://vuejs.org/api/options-state.html#computed)  
- [https://vuejs.org/guide/essentials/computed.html#basic-example](https://vuejs.org/guide/essentials/computed.html#basic-example)

계산된 프로퍼티가 필요할 때 쓰는 getter/setter라고 보면 됨. 캐시도 적용된다 함.

```js
export default {
  data() {
    return { a: 1 }
  },
  computed: {
    // readonly
    aDouble() {
      return this.a * 2
    },
    // writable
    aPlus: {
      get() {
        return this.a + 1
      },
      set(v) {
        this.a = v - 1
      }
    }
  },
  created() {
    console.log(this.aDouble) // => 2
    console.log(this.aPlus) // => 2

    this.aPlus = 3
    console.log(this.a) // => 2
    console.log(this.aDouble) // => 4
  }
}
```

`computed`는 `data()`나 `methods`보다 실행이 늦기 때문에 초기화 시점에 참조하는 것은 불가능하다.


## watch

컴포넌트 프로퍼티의 변화를 감시할 때 사용함:

```js
createApp({
  // ... 생략
  watch: {
    todoId() {
      this.fetchData()
    }
  }
}).mount('#app')
```

```js
createApp({
  // ... 생략
  watch: {
    someObject: {
      handler(newValue, oldValue) {
        // Note: `newValue` will be equal to `oldValue` here
        // on nested mutations as long as the object itself
        // hasn't been replaced.
      },
      deep: true
    }
  }
});
```


## expose

컴포넌트 인스턴스의 속성 중 부모에게 노출할 속성을 명시적으로 제한하기 위해 사용한다. expose가 없으면 모두 공개 상태가 되고, expose가 있으면 명시한 것만 공개된다. 만약 모두 숨기고 싶을 땐 빈 배열을 할당한다.

```js
// 부모는 이 컴포넌트의 getCodes만 보임
expose: ['getCodes'],
methods: {
  initialize() {
    ...
  },
  getCodes() {
    ...
  }
}
```


## emits

컴포넌트에서 내보낼 커스텀 이벤트를 선언한다.

TODO