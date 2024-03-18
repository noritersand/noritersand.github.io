---
layout: post
date: 2022-02-07 00:16:51 +0900
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

#### 참고 문서

- [React \| Built-in React Hooks](https://react.dev/reference/react/hooks)

#### 테스트 환경 정보

- React 18


## 개요

리액트 훅에 대해 정리한 문서


## 훅 호출 규칙

[React | Meet your first Hook ](https://react.dev/learn/state-a-components-memory#meet-your-first-hook)

- 훅은 컴포넌트의 최상위 수준 또는 커스텀 훅에서만 호출할 수 있다.
- 조건문이나 반복문 혹은 기타 중첩 함수 내부에서는 호출할 수 없다.
- 리액트 함수가 아닌 일반적인 자바스크립트 함수에서는 호출할 수 없다.

요약하면 컴포넌트를 정의하는 함수의 가장 상위 스코프와 커스텀 훅에서만 호출 가능하다는 것. 위반하면 이런 에러 메시지를 얻게 된다:

```js
Uncaught Error: Invalid hook call. Hooks can only be called inside of the body of a function component. This could happen for one of the following reasons:
1. You might have mismatching versions of React and the renderer (such as React DOM)
2. You might be breaking the Rules of Hooks
3. You might have more than one copy of React in the same app
See https://reactjs.org/link/invalid-hook-call for tips about how to debug and fix this problem.
```

VSCODE를 쓴다면 [ESLint `dbaeumer.vscode-eslint`](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)를 설치해서 규칙 위반을 확인할 수 있다.


## 훅 목록

- State Hooks
  - useState
  - useReducer
- Context Hooks
  - useContext
- Ref Hooks
  - useRef
  - useImperativeHandle
- Effect Hooks
  - useEffect
  - useLayoutEffect
  - useInsertionEffect
- Performance Hooks 
  - useMemo
  - useCallback
  - useTransition
  - useDeferredValue
- Resource Hooks
  - use
- Other Hooks
  - useDebugValue
  - useId
  - useSyncExternalStore

그리고 개발자가 정의하는 커스텀 훅이 있다.


## 스테이트 훅 State Hooks

스테이트 훅은 **TODO**

### useState

Reactive state를 정의하는 훅.

```
const [state, modifier] = React.useState(initialState);
```

- `state`: Reactive state, 줄여서 상태 혹은 state. 리액트는 이 값이 변화할 때 자동으로 다시 렌더링한다(다른 말로, *state의 변화는 리렌더링을 유발한다*).
- `initialState`: `state`의 초기값.
- `modifier`: `state`의 값을 변경하기 위해 호출하는 함수. 이 함수의 이름은 state 변수의 이름 앞에 `set`을 붙이는 게 관행적이다. (따라서 위 예시에선 `setState()`가 되어야 한다)

```jsx
const [counter, setCounter] = React.useState(0);
const onClick = () => setCounter(prev => prev + 1);
return (
  <div>
    <h3>Total clicks: {counter}</h3>
    <button type="button" onClick={onClick}>Click Me</button>
  </div>
);
```

#### state 값 변경하기

state의 값은 직접 재할당하는 게 아니라 modifier를 통해서 리액트에 해당 컴포넌트와 관련 컴포넌트들이 다시 렌더링해야 한다고 알리는 방식을 쓴다:

```jsx
// 원시 타입
const [value, setValue] = useState('');
const handleEvent1 = event => {
  // value = 'abc'; // X
  setValue('abc'); // O
};

// 배열
const [values, setValues] = useState([]);
const handleEvent2 = event => {
  // values = [1, 2, 3]; // X
  setValues([1, 2, 3]); // O
};

// 객체
const [obj, setObj] = React.useState({someFlag: false});
const handleEvent3 = event => {
  // obj.someFlag = true; // X
  setObj({someFlag: true}); // O
};
```

#### 현재 state를 기반으로 업데이트하기

리액트는 state 값의 변경을 비동기로 처리한다. 따라서 다음처럼 현재 state를 기반으로 값을 업데이트 하는 코드는 타이밍 문제가 발생한다:

```jsx
const [counter, setCounter] = React.useState(0);
const onClick = () => setCounter(counter + 1); // X
```

대신 아래처럼 현재값을 받아와 계산하는 코드를 리액트가 실행하도록 콜백 함수로 전달해야 한다:

```jsx
setCounter(current => current + 1); // O
// setCounter((current) => { return current + 1 });
```

이 콜백 함수를 *updater function* 혹은 *functional update*라고 부른다.

만약 state가 배열이고 새 요소를 덧붙이려면 다음처럼 작성한다:

```jsx
const [values, setValues] = useState([]);

const handleEvent2 = event => {
  let value = event.target.value;
  // setValues(values.push(value)); // 잘못된 방법
  setValues(prev => [...prev, value]); // 이렇게 하거나
  // setValues([...values, value]); // 이렇게 해도 됨
};
```

#### state 변경 감지하기

modifier는 비동기적으로 작동하기 때문에 modifier 호출 직후 state를 읽는 코드는 문제를 일으킬 수 있다.

\* 좀 더 정확히 표현하면, 리액트는 컴포넌트가 다시 렌더링 될 때까지 state의 값을 갱신하지 않는다. 이 글을 보자 [React GitHub | this.state는 왜 즉시 갱신되지 않는가?](https://github.com/facebook/react/issues/11527#issuecomment-360199710)  

```jsx
const [obj, setObj] = React.useState({someFlag: false});

setObj({someFlag: true});
console.log(obj); // Object { someFlag: false }
```

대신 `useEffect` 훅을 사용하여 특정 state가 업데이트 됐을 때를 감지하도록 해야 한다:

```jsx
const [obj, setObj] = React.useState({someFlag: false});

const handleEvent = (event) => {
  setObj({someFlag: true});
};

useEffect(() => {
  console.log(obj);
}, [obj]);
```

그런데 이렇게 하면 최초 렌더링 때 발생하는 컴퓨터 이벤트에도 `useEffect` 훅이 작동한다. 최초 렌더링에 반응하지 않으려면 `useRef` 훅을 이용하는 방법이 있다(점점 산으로 간다 😟):

```jsx
const [obj, setObj] = React.useState({someFlag: false});
const isMounted = React.useRef(false);

const handleEvent = (event) => {
  setObj({someFlag: true});
};

useEffect(() => {
  if (isMounted.current) { // 최초 렌더링 때는 초기값인 false로 설정되어 있음
    console.log('obj:', obj);
  } else {
    isMounted.current = true;
  }
}, [obj]);
```

### useReducer

[React | Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

**TODO** reducer로 복잡한 state 관리를 덜어낼 수 있다고 한다. 반드시 알아볼 것


## 컨텍스트 훅 Context Hooks

### useContext

**TODO**


## 레퍼런스 훅 Ref Hooks

### useRef

`useRef()`는 주어진 값으로 초기화된 `.current` 프로퍼티를 소유한 객체를 반환한다. 보통 값의 변경은 `.current`를 이용하며 **이 행동은 렌더링을 유발하지 않는다**.

```
const ref = React.useRef(initialValue)
```

- `initialValue`: `ref.current`의 초기값

```js
const rf = useRef("멋에쓰는물건인고");
console.log(rf); // Object { current: "멋에쓰는물건인고" }
```

`<div ref={myRef} />` 이런식으로 작성하면 리액트는 렌더링 될 때마다 변경된 DOM 노드를 `ref`로 전달한다. 매 렌더링마다 항상 동일한 객체를 제공한다고 한다.

보통 리렌더링 없이 재할당하거나 DOM 객체가 필요할 때 사용한다. 아래는 `<button>`이 클릭될 때마다 `focused.current`의 객체가 바뀌는 코드다:

```jsx
function UseRefTest() {
  const [focusSwitch, setFocusSwitch] = React.useState(true);
  const focused = React.useRef(null);

  const onButtonClick = () => {
    setFocusSwitch(!focusSwitch);
    console.log('focused.current:', focused.current);
  };

  const arr = [true, false];

  return (
    <>
      {
        arr.map(item => {
          return (
            <div ref={item === focusSwitch ? focused : null}
                className={item === focusSwitch ? 'active' : ''}>phew-phew!</div>
          );
        })
      }
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```


## 이펙트 훅 Effect Hooks

이펙트 훅은 effect(이하 이펙트)를 통해 컴포넌트를 *외부 시스템*(네트워크, 브라우저 DOM, 애니메이션 등 리액트가 제어하기 않는 모든 코드)과 연결하고 동기화하는데 사용한다. 

**FIXME** 여기서 이펙트란, 렌더링 자체에 의해 발생하는 부수 효과를 의미하며 일반적은 프로그래밍의 부수 효과와는 다르다. 아마 사용자에 의해 발생하는 어떤 트리거를 이벤트라고 한다면, 어떤 상호 작용 없이도 발생할 수 있는 트리거를 이펙트라고 하는 모양.

### useEffect

공식 도움말에서는 *컴포넌트를 외부 시스템에 연결한다(connects a component to an external system)*라고 설명한다. 또 다른 설명에서는 *리액트 컴포넌트가 렌더링된 후 부수 효과를 실행하기 위한 훅*이라고 한다.

```
React.useEffect(setup)
React.useEffect(setup, dependencies)
```

- `setup`: 이펙트 로직을 포함하는 콜백 함수. 함수의 바디는 컴포넌트 마운트 시점에 실행될 설정 코드(setup code)라고 부른다. 이 함수는 또 다른 함수를 반환할 수 있는데 이것을 정리 코드(cleanup code)라고 부르며, 의존하는 반응형 값들이 변경되었거나 컴포넌트가 화면에서 제거된 이후 실행된다.

- `dependencies`: `setup`의 내부에서 참조되는 모든 반응형 값들(reactive values)의 배열. 줄여서 의존성 배열(list of dependencies)이라 함.

반환값은 `undefined`다.

```jsx
// 코드 출처: https://react.dev/reference/react/useEffect#connecting-to-an-external-system
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    // 설정 코드
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect(); // 정리 코드
    };
  }, [serverUrl, roomId]); // 의존성 배열
  // ...
}
```

의존성 배열을 어떻게 지정했느냐에 따라 코드 실행 주기가 달라진다:

```jsx
useEffect(() => {
  // 이 코드는 렌더링 때마다 실행된다.
});

useEffect(() => {
  // 이 코드는 마운트 시에만 딱 한 번 실행된다.
}, []);

useEffect(() => {
  // 이 코드는 마운트 시점과 a 혹은 b가 변경된 경우에 실행된다.
}, [a, b]);
```

이런 특징을 이용해서 원래의 목적(외부 시스템과 연결)을 벗어나 코드 실행 주기를 제어하기 위해 사용하기도 한다.

🚨 리액트는 개발모드에서 설정 코드를 실행하기 전에 [버그 탐지와 검증을 위해 설정 코드와 정리 코드를 미리 한 번 더 실행](https://react.dev/learn/synchronizing-with-effects#step-3-add-cleanup-if-needed)시킨다. 따라서 개발 모드에서 `useEffect()`에 작성한 콘솔 출력은 두 번 작동할 것이다.

🚧 불필요한 이펙트 훅은 코드를 복잡하게 하고 실행 속도를 느려지게 만든다. 이 [문서](https://react.dev/learn/you-might-not-need-an-effect)를 참고할 것.


## 퍼포먼스 훅 Performance Hooks 

### useMemo

**TODO**
