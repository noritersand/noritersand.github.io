---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[React] 리액트 훅 React Hooks'
categories:
  - react
tags:
  - react
  - javascript
  - hook
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[React\] Hook의 개요](https://ko.reactjs.org/docs/hooks-intro.html)

#### 버전 정보

- React 17

## 개요

리액트 16.8에 새로 추가된 훅 내용 정리.

## 훅 목록

기본 훅:

- State Hook
- Effect Hook
- Context Hook

추가 훅:

- Reducer
- Callback
- Memo
- Ref
- ImpreativeHandle
- LayoutEffect
- DebugValue

## [훅 호출 규칙](https://ko.reactjs.org/docs/hooks-rules.html)

- 훅 호출은 최상위 레벨에서: 반복문, 조건문 혹은 nested 함수에서 훅 호출하면 안됨
- 훅 호출은 리액트 함수(컴포넌트 혹은 커스텀 훅) 내에서만: 일반적인 자바스크립트 함수에서 호출하면 안됨

요약하면 컴포넌트를 정의하는 함수의 가장 상위 스코프에서만 호출하라는 것.

VSCODE를 쓴다면 [ESLint `dbaeumer.vscode-eslint`](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)를 설치해서 규칙 위반을 확인할 수 있다.

## useState

```
const [state, setState] = useState(initialState);
```

TODO

## useEffect

TODO

## useContext

TODO

## useRef

```
const refContainer = useRef(initialValue);
```

`useRef()`는 주어진 값으로 초기화된 `.current` 프로퍼티를 소유한 객체를 반환한다. 보통 값의 변경은 `.current`를 이용하며 이 행동은 렌더링을 유발하지 않는다.

```js
const rf = useRef("멋에쓰는물건인고");
console.log(rf); // Object { current: "멋에쓰는물건인고" }
```

<!-- TODO: 이 부분이 필요한가? -->
<!-- ```js
rf.current = '야';
console.log(rf); // Object { current: "야" }

rf = '왜'; // Uncaught TypeError: invalid assignment to const 'rf'
``` -->

`useRef()`로 만들어지는 객체는 순수 자바스크립트 객체이며 매 렌더링마다 항상 동일한 ref 객체를 제공한다고 한다.

그리고 보통 이렇게 쓰인다 함:

```js
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
    console.log(inputEl.current); // <input type="text">
    console.log(inputEl.current === document.querySelector('input')); // true
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

`<div ref={myRef} />`와 같은 방식으로 ref 객체를 전달하면 리액트는 모드가 변경될 때마다 변경된 DOM 노드를 전달한다는디... 뭔소리람

TODO
