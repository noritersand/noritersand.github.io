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

{% raw %}

#### 참고 문서

- [React \| Built-in React Hooks](https://react.dev/reference/react/hooks)

#### 테스트 환경 정보

- React 18


## 개요

리액트 훅에 대해 정리한 문서


## 훅 호출 규칙

[React \| Meet your first Hook ](https://react.dev/learn/state-a-components-memory#meet-your-first-hook)

- 훅은 컴포넌트의 최상위 수준(=스코프, 유효범위) 또는 커스텀 훅에서만 호출할 수 있다.
- 조건문이나 반복문 혹은 기타 중첩 함수 내부에서는 호출할 수 없다.
- 리액트 함수가 아닌 일반적인 자바스크립트 함수에서는 호출할 수 없다.

요약하면 컴포넌트를 정의하는 함수의 가장 상위 스코프와 커스텀 훅에서만, 그리고 그 안에서도 제어문 바깥 지역에서만 호출 가능하다는 것. 위반하면 이런 에러 메시지를 얻게 된다:

```js
Uncaught Error: Invalid hook call. Hooks can only be called inside of the body of a function component. This could happen for one of the following reasons:
1. You might have mismatching versions of React and the renderer (such as React DOM)
2. You might be breaking the Rules of Hooks
3. You might have more than one copy of React in the same app
See https://reactjs.org/link/invalid-hook-call for tips about how to debug and fix this problem.

// 혹은

ESLint: React Hook "useCallback" is called conditionally. React Hooks must be called in the exact same order in every component render. 
Did you accidentally call a React Hook after an early return?(react-hooks/ rules-of-hooks)
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
- Custom Hooks


## 스테이트 훅 State Hooks

스테이트 훅은 **TODO**

### useState

Reactive state를 정의하는 훅.

```
const [state, modifier] = React.useState(initialState);
```

- `initialState`: `state`의 초기값.
- `state`: `useState`가 반환한 Reactive state(줄여서 상태 혹은 state). 리액트는 이 값이 변화할 때 자동으로 다시 렌더링한다. 그러니까 state의 변화는 리렌더링을 유발한다.
- `modifier`: `state`의 값을 변경하기 위해 호출하는 함수. 이 함수의 이름은 state 변수의 이름 앞에 `set`을 붙이는 게 관행적이다. (따라서 위 예시에선 `setState()`가 되어야 한다)

```jsx
import {useState} from 'react';

const [counter, setCounter] = useState(0);
const onClick = () => setCounter(prev => prev + 1);
return (
  <div>
    <h3>Total clicks: {counter}</h3>
    <button type="button" onClick={onClick}>
      Click Me
    </button>
  </div>
);
```

#### state 값 변경하기

state의 값은 직접 재할당하는 게 아니라 modifier를 통해서 리액트에 해당 컴포넌트와 관련 컴포넌트들이 다시 렌더링해야 한다고 알리는 방식을 쓴다:

```jsx
import {useState} from 'react';

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
const [obj, setObj] = useState({someFlag: false});
const handleEvent3 = event => {
  // obj.someFlag = true; // X
  setObj({someFlag: true}); // O
};
```

#### 현재 state를 기반으로 업데이트하기

리액트는 state 값의 변경을 비동기로 처리한다. 따라서 다음처럼 현재 state를 기반으로 값을 업데이트 하는 코드는 타이밍 문제가 발생한다:

```jsx
import {useState} from 'react';

const [counter, setCounter] = useState(0);
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
import {useState} from 'react';

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

\* 좀 더 정확히 표현하면, 리액트는 컴포넌트가 다시 렌더링 될 때까지 state의 값을 갱신하지 않는다. 이 글을 보자 [React GitHub \| this.state는 왜 즉시 갱신되지 않는가?](https://github.com/facebook/react/issues/11527#issuecomment-360199710)

```jsx
import {useState} from 'react';

const [obj, setObj] = useState({someFlag: false});

setObj({someFlag: true});
console.log(obj); // Object { someFlag: false }
```

대신 `useEffect` 훅을 사용하여 특정 state가 업데이트 됐을 때를 감지하도록 해야 한다:

```jsx
import {useState} from 'react';

const [obj, setObj] = useState({someFlag: false});

const handleEvent = event => {
  setObj({someFlag: true});
};

useEffect(() => {
  console.log(obj);
}, [obj]);
```

그런데 이렇게 하면 최초 렌더링 때 발생하는 컴퓨터 이벤트에도 `useEffect` 훅이 작동한다. 최초 렌더링에 반응하지 않으려면 `useRef` 훅을 이용하는 방법이 있다:

```jsx
import {useRef, useState} from 'react';

const [obj, setObj] = useState({someFlag: false});
const isMounted = useRef(false);

const handleEvent = event => {
  setObj({someFlag: true});
};

useEffect(() => {
  if (isMounted.current) {
    // 최초 렌더링 때는 초기값인 false로 설정되어 있음
    console.log('obj:', obj);
  } else {
    isMounted.current = true;
  }
}, [obj]);
```

### useReducer

[React \| Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

```
const [state, dispatch] = React.useReducer(reducer, initialArg, init?)
```

- `reducer`: 상태를 업데이트할 함수 객체. 이 함수는 첫 번째 매개변수로 `state`, 두 번째 매개변수로 `action`을 갖는 순수 함수여야 하며 `state`를 반환해야 한다.
- `initialArg`: `state` 초기값. 모든 데이터 타입을 할당할 수 있따.
- `init`: 초기화 함수. 이 함수의 반환값이 `state`의 초기값으로 설정된다. 생략하면 `state`의 초기값은 `initialArg`와 같다.
- `state`: 상태값을 저장하는 변수. `useState`의 `state`와 개념은 같다
- `dispatch`: 상태값을 업데이트할 때 호출하는 함수로, 이 함수의 호출은 리렌더링을 유발한다.

`useReducer`는 `useState`를 대체할 수 있는 훅으로 복잡한 상태를 관리하거나 구조화된 상태 전환 로직이 필요할 때 사용한다. 상태와 로직을 분리하여 가독성을 높인다거나 액션 기반(미리 정해진 키워드로 상태를 업데이트)으로 상태를 업데이트할 때 사용하기도 한다.

```jsx
import {useReducer} from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error('Unknown action type');
  }
}

export default function TestUseReducer1() {
  const [state, dispatch] = useReducer(reducer, {count: 0});

  return (
    <article>
      <p>state.count: {state.count}</p>
      <button onClick={() => dispatch({type: 'increment'})}>Increment</button>
      <br />
      <button onClick={() => dispatch({type: 'decrement'})}>Decrement</button>
    </article>
  );
}
```

`dispatch`는 내부에서 `reducer`를 호출하는데 이 때 첫 번째 인수로 이전 상태값을, 두 번째 인수로 `dispatch()`의 매개변수를 그대로 전달한다. 

아래는 객체 타입의 상태를 관리하는 코드다:

```jsx
import {useReducer} from 'react';

const initialState = {
  name: '',
  age: 0
};

// 리듀서 함수
function reducer(state, action) {
  switch (action.keyName) {
    case 'name':
      return {...state, name: action.payload};
    case 'age':
      return {...state, age: action.payload};
    case 'reset':
      return initialState;
    default:
      throw new Error('Unknown action keyName');
  }
}

export default function TestUseReducer2() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <article>
      <div>
        <label>
          Name:
          <input
            type="text"
            value={state.name}
            onChange={e => dispatch({keyName: 'name', payload: e.target.value})}
          />
        </label>
      </div>
      <div>
        <label>
          Age:
          <input
            type="number"
            value={state.age}
            onChange={e => dispatch({keyName: 'age', payload: Number(e.target.value)})}
          />
        </label>
      </div>
      <div>
        <button onClick={() => dispatch({keyName: 'reset'})}>Reset</button>
      </div>
      <div>
        <p>Name: {state.name}</p>
        <p>Age: {state.age}</p>
      </div>
    </article>
  );
}
```


## 컨텍스트 훅 Context Hooks

컨텍스트(context)를 다루기 위한 훅이다. 현재(2024-03-29)는 `useContext` 하나만 제공하며, 컨텍스트란 일반적인 컴포넌트 유효 범위를 넘어서는 state 정도로 해석된다.

### useContext

- [React \| useContext}(https://react.dev/reference/react/useContext)
- [React \| createContext}(https://react.dev/reference/react/createContext)

```
useContext(SomeContext)
```

- `SomeContext`: `createContext()`로 생성한 컨텍스트 객체다. 컨텍스트 객체 자체는 아무 정보도 갖고 있지 않지만, 컴포넌트 트리 안에서 데이터를 전역적으로 공유하기 위한 메커니즘을 제공한다.

`useContext`는 컴포넌트의 깊이와 상관없이 컴포넌트 간 정보를 주고 받기 위한 훅이다. 사용 방법을 요약하면, `createContext()`로 컨텍스트 객체를 생성하고, `Provider`로 컨텍스트에 값을 전달하며 `useContext()`로 가져오는 방식이다.

```jsx
// test-use-context.jsx
import {createContext, useState} from 'react';
import Button from 'components/button';
import Paragraph from 'components/paragraph';

export const Foo = createContext({});

export default function TestUseContext() {
  const [count, setCount] = useState(0);
  const increment = () => setCount(prev => prev + 1);
  return (
    <article>
      <h2>useContext</h2>
      <Foo.Provider value={{count, increment}}>
        <div>
          <Button>이 버튼이나</Button>
        </div>
        <div>
          <Button>이 버튼을 누르면 값이 증가함</Button>
        </div>
        <div>
          <Paragraph />
        </div>
      </Foo.Provider>
      <Button>여긴 안됨</Button>
    </article>
  );
}
```

```jsx
// button.jsx
import {useContext} from 'react';
import {Foo} from 'pages/test-use-context';

export default function Button({children}) {
  const {increment} = useContext(Foo);
  return (
    <button type="button" onClick={increment}>
      {children}
    </button>
  );
}
```

```jsx
// paragraph.jsx
import {useContext} from 'react';
import {Foo} from 'pages/test-use-context';

export default function Paragraph() {
  const {count} = useContext(Foo);
  return <p>click count: {count}</p>;
}
```

도움말에서는 `useContext()`를 호출하는 컴포넌트를 consumer라 표현하는데, 실제로 이전 버전에선 `.Consumer` 프로퍼티를 통해 읽어왔다고 한다:

```jsx
function Button() {
  // 🟡 이전 방식 (권장하지 않음)
  return (
    <ThemeContext.Consumer>
      {theme => (
        <button className={theme} />
      )}
    </ThemeContext.Consumer>
  );
}
```

⚠️ provider가 공유한 값은 오직 하위 컴포넌트에서만 접근할 수 있다. 만약 아래처럼 컴포넌트를 provider 바깥에서 선언하면 `useContext()`는 빈 객체`{}`를 반환한다:

```jsx
export default function TestUseContext() {
  // 생략
  return (
    <article>
      <h2>useContext</h2>
      <Foo.Provider value={{count, increment}}>
        <>...</>
      </Foo.Provider>
      <Button>여긴 안됨</Button>
    </article>
  );
}
```

```jsx
export default function Button({children}) {
  const context = useContext(Foo);
  console.log(Object.keys(context).length === 0); // true
  // 생략
}

```


## 레퍼런스 훅 Ref Hooks

### useRef

`useRef()`는 주어진 값으로 초기화된 `.current` 프로퍼티를 소유한 객체를 반환한다.

```
const ref = React.useRef(initialValue)
```

- `initialValue`: 초기값

반환값은 `.current` 프로퍼티 하나만 있는 객체다. `initialValue`가 `.current` 프로퍼티의 초기값으로 할당된다.

```js
const rf = useRef('멋에쓰는물건인고');
console.log(rf); // Object { current: "멋에쓰는물건인고" }
```

#### 렌더링을 유발하지 않는 별도의 상태값

`useRef` 값의 변경은 `.current` 프로퍼티를 통해 재할당한다. 이 행동은 렌더링을 유발하지 않으며, 한 번 할당된 값은 (일부러 재할당 하지 않는 이상) 컴포넌트의 리렌더링이 발생해도 초기화되지 않는다. 이 특성을 이용해 별도의 저장공간으로 활용되곤 한다.

아래의 코드는 상태값이 변경되어도 `useEffect`에서 할당한 값이 변화하지 않는 것을 보여준다:

```jsx
import {useEffect, useRef, useState} from 'react';

export default function App() {
  console.log('App render');
  const refTest = useRef(null);
  const [value, setValue] = useState('');

  useEffect(() => {
    refTest.current = 123; // 첫 렌더링에 123 할당
  }, []);

  const onChange = event => setValue(event.target.value);

  console.log(refTest.current); // 첫 렌더링엔 null, 이후 사용자 입력이 발생하면 123 출력됨

  return <input type="text" value={value} onChange={onChange} />;
}
```

#### DOM 객체에 연결

DOM 객체에 직접 접근이 필요할 때에도 `useRef`를 사용한다. 

아래는 버튼을 눌렀을 때 `<input>`에 포커싱하는 코드다:

```jsx
import {useRef} from 'react';

export default function App() {
  const myRef = useRef(null);
  const focusInput = () => myRef.current.focus();

  return (
    <div>
      <input type="text" ref={myRef} />
      <button onClick={focusInput}>focus input</button>
    </div>
  );
}
```

ℹ️ 리액트는 렌더링 때마다 변경된 DOM 노드를 `ref`로 전달한다고 한다.

#### forwardRef

`React.forwardRef()` 함수는 `ref` 객체를 자식 컴포넌트의 DOM 객체와 연결할 때 사용한다.

```
const SomeComponent = forwardRef(renderFn)

function renderFn(props, ref) {}
```

- `renderFn`: 
  - `props`: 부모 컴포넌트로부터 전달 받은 props
  - `ref`: 부모 컴포넌트로부터 전달 받은 `ref`

반환값은 React 컴포넌트고, 이 컴포넌트가 `renderFn()`을 실행한다.

```jsx
import {forwardRef, useRef} from 'react';
import styles from '../style/forward-ref.module.css';

// 자식 컴포넌트
const ChildComponent = forwardRef((props, ref) => {
  return (
    <div>
      <h3>Child Component</h3>
      <input type="text" ref={ref} />
    </div>
  );
});
ChildComponent.displayName = 'ChildComponent'; // ESLint 에러 방지용 display name 설정

// 부모 컴포넌트
export default function ParentComponent() {
  const inputRef = useRef(null);

  const handleClick = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <div className={styles.childContainer}>
        <ChildComponent ref={inputRef} />
      </div>
      <button onClick={handleClick}>Focus Input</button>
    </div>
  );
}
```


## 이펙트 훅 Effect Hooks

이펙트 훅은 이펙트(effect)를 통해 컴포넌트를 *외부 시스템*(네트워크, 브라우저 DOM, 애니메이션 등 리액트가 제어하기 않는 모든 코드)과 연결하고 동기화하는데 사용한다. 

**FIXME** 여기서 이펙트란, 렌더링 자체에 의해 발생하는 부수 효과를 의미하며 일반적은 프로그래밍의 부수 효과와는 다르다. 아마 사용자에 의해 발생하는 어떤 트리거를 이벤트라고 한다면, 어떤 상호 작용 없이도 발생할 수 있는 트리거를 이펙트라고 하는 모양.

### useEffect

공식 도움말에서는 *컴포넌트를 외부 시스템에 연결한다(connects a component to an external system)*라고 설명한다. 또 다른 설명에서는 *리액트 컴포넌트가 렌더링된 후 부수 효과를 실행하기 위한 훅*이라고 한다.

```
React.useEffect(setup)
React.useEffect(setup, dependencies)
```

- `setup`: 이펙트 로직을 포함하는 콜백 함수. 함수의 바디는 컴포넌트 마운트 시점에 실행되며 설정 코드(setup code)라 부른다. 이 함수는 또 다른 함수를 반환할 수 있는데 이것은 정리 코드(cleanup code)라 부르며, 의존하는 반응형 값들이 변경되었거나 컴포넌트가 화면에서 제거된 이후 실행된다.
- `dependencies`: `setup`의 내부에서 참조되는 모든 반응형 값들(reactive values)의 배열. 줄여서 의존성 배열(list of dependencies)이라 함.

반환값은 `undefined`다.

```jsx
// 코드 출처: https://react.dev/reference/react/useEffect#connecting-to-an-external-system
import {useEffect} from 'react';
import {createConnection} from './chat.js';

function ChatRoom({roomId}) {
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

🚧 불필요한 이펙트 훅 사용은 코드를 복잡하게 하고 실행 속도를 느려지게 만든다. [이 문서](https://react.dev/learn/you-might-not-need-an-effect)를 참고할 것.

ℹ️ 개발환경에서 Strict Mode(ECMAScript의 엄격 모드와 다름)가 활성화되어 있다면, 리액트는 버그 탐지와 검증을 위해 [설정 코드(setup code)를 실행하기 전에 설정 코드와 정리 코드를 미리 한 번 더 실행한다](https://react.dev/reference/react/useEffect#my-effect-runs-twice-when-the-component-mounts).

ℹ️ 정리 코드(cleanup code)가 필요한 이유는 [이 문서](https://react.dev/learn/synchronizing-with-effects#step-3-add-cleanup-if-needed)를 보자.


## 퍼포먼스 훅 Performance Hooks 

불필요한 렌더링을 방지하고 성능을 최적화하는 데 사용되는 훅이다.

🚨 퍼포먼스 훅은 참조하는 변수뿐만 아니라 참조하는 함수 객체와 클로저까지 고려하며 사용해야 하는 훅이다. 생각 없이 마구 남발하면 온세상이 버그 천지다. 🥲

### useMemo

계산된 값을 메모이제이션(memoization, 동일한 입력이면 저장해 둔 결과를 재사용하는 것)하여 리렌더링간에 재사용한다.

```
const cachedValue = React.useMemo(fn, dependencies)
```

- `fn`: 캐싱하려는 값을 계산하는 함수 객체. 순수 함수여야 하며 인자를 받지 않아야 한다. 지난 렌더링에서 `dependencies`가 변경되지 않으면 동일한 값을 다시 반환한다. `dependencies`가 변경되면 이 함수를 다시 호출하여 반환하고, 다음에 재사용할 수 있도록 저장한다.
- `dependencies`: `fn` 내에서 참조되는 모든 반응형 값의 목록.

반환값은 `fn` 함수가 반환한 값이다.

아래 예시를 실행해 보면 의존성 배열에 없는 `value3`이 변경될 때만 `useMemo`가 실행되지 않는 것을 확인할 수 있다:

```jsx
import {useMemo, useState} from 'react';

export default function TestUseMemo() {
  console.log('TestUseMemo 렌더링됨');
  const [value1, setValue1] = useState(0);
  const [value2, setValue2] = useState(0);
  const [value3, setValue3] = useState(0);

  const cachedValue = useMemo(() => {
    console.log('useMemo 실행됨');
    let complexValue = Number(value1) + Number(value2);
    return complexValue;
  }, [value1, value2]);

  const rerender = () => {
    setValue3(prev => ++prev);
  };

  return (
    <article>
      <h2>useMemo</h2>
      <div>
        <input value={value1} onChange={e => setValue1(e.target.value)} />
        <input value={value2} onChange={e => setValue2(e.target.value)} />
        <p>{cachedValue}</p>
      </div>
      <button onClick={rerender}>리렌더링</button>
    </article>
  );
}
```

### useCallback

함수 정의를 캐싱하여 리렌더링간에 재사용이 가능하도록 해주는 훅이다. 함수형 컴포넌트는 렌더링될 때마다 내부의 모든 함수를 새로 생성하는데, `useCallback`을 사용하면 함수를 메모이제이션하여 입력이 동일하면 이전에 생성한 함수를 재사용하는 식으로 작동한다.

ℹ️ 값을 메모이제이션 하는 `useMemo`와 달리, `useCallback`은 함수 객체를 메모이제이션 한다는 차이가 있다.

```
React.useCallback(fn, dependencies)
```

- `fn`: 캐싱할 함수 겍체. 다음 렌더링에서 `dependencies`의 값이 전과 같다면 동일한 함수를 반환한다.
- `dependencies`: **`fn` 내에서 참조되는 모든 반응형 값의 목록**. 빈 배열을 할당하면, `fn`은 첫 렌더링 시에만 생성되고, 이후 동일한 함수가 계속 반환된다.

최초 렌더링에서는 전달한 `fn`을 그대로 반환하지만, 다음 렌더링부터는 의존하는 값의 변화에 따라 이전 렌더링에 저장한 `fn` 함수를 반환하거나, 현재 렌더링 중 전달한 `fn` 함수를 반환한다.

```js
const memoizedCallback = useCallback(
  () => {
    // 콜백 함수의 내용
  },
  [/* 의존성 배열 */]
);
```

```js
import React, { useCallback } from 'react';

const MyComponent = () => {
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []);

  return <MyChildComponent onClick={handleClick} />;
};
```

```js
// 코드 출처: https://react.dev/reference/react/useCallback
import { useCallback } from 'react';

export default function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);

  return (
    // ...
  )
}
```

⚠️ 앞에서 의존성 배열에 대해 '`fn` 내에서 참조되는 모든 반응형 값의 목록'이라 했다. 이 말은 장난이 아니라 반드시 함수 내부에서 참조하는 모든 reactive state를 의존성 배열에 추가해야 한다. 그렇지 않으면 함수에서 참조하는 state는 최신 값으로 갱신되지 않는다. 예를 들어:

```js
const [foo, setFoo] = useState(0);
const [refresh, setRefresh] = useState(true);

const callback = useCallback(() => {
  return foo;
}, [refresh]);
```

이렇게 작성한 함수 `callback()`은 `refresh` 값이 변경될 때까지 `foo`의 초기값인 `0`을 반환하게 된다.

#### 🚨 useCallback의 잘못된 사용 사례

```jsx
import {useCallback, useEffect, useRef, useState} from 'react';

export default function UseCallbackWrongUsages() {
  const [foo, setFoo] = useState('');
  const [flag, setFlag] = useState(false);

  function featureFunction() {
    setFlag(foo === '');
  }

  function bridge() {
    featureFunction();
  }

  const wrongFunction = useCallback(() => {
    bridge();
  }, []); // <-- 문제를 해소하려면 의존성 배열로 foo가 추가되어야 함

  useEffect(() => {
    console.log('rendered');
    wrongFunction();
  }, [foo])

  return (
    <>
      <section>
        <h2>useCallback의 잘못된 사용 사례</h2>
        <section>
          <h3>함수의 클로저</h3>
          <div className="code-result">
            <input type="text" value={foo} onChange={e => setFoo(e.target.value)} /><br />
            <p>foo는 빈 문자열인가? {flag ? '예' : '아니오'}</p>
          </div>
        </section>
      </section>
    </>
  );
}
```

이 코드에서 `flag`는 항상 `true`다. 

`useCallback` 훅으로 함수를 생성하면 생성 시점의 참조값을 *클로저로 캡처*한다. 캡처된 값은 함수를 다시 생성할 때까지 변하지 않는다. (원래 클로저는 변수를 참조하기만 하지 그 값을 고정하거나 캡처하지 않는다.)

`wrongFunction()`은 `useCallback`으로 생성되었고 처음 렌더링될 때 `foo`의 값을 클로저로 캡처하게 된다. 하지만 의존성 배열이 비어있어서 처음 한 번만 생성된다. 그 결과 `foo`가 변경되어도 `wrongFunction()`과 캡처된 값은 변경되지 않는다.


## 커스텀 훅 Custom Hooks

커스텀 훅이란 개발자가 정의한 훅으로, 리액트가 제공하는 기본 훅들을 내부에서 호출하는 로직이 포함된 함수를 의미한다.

간단한 예로 아래처럼 작성할 수 있다:

```jsx
import {useState} from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);

  return {count, increment, decrement, reset};
}

export default useCounter;
```

ℹ️ 커스텀 훅에서 생성한 상태값은 커스텀 훅을 사용한 컴포넌트 내부 상태로 관리된다.

### 예시: 로그인, 로그아웃 함수와 로그인 상태를 컨텍스트로 제공하는 커스텀 훅

```jsx
import React, {createContext, useContext, useEffect, useState} from 'react';

const AuthContext = createContext(undefined);

export const AuthProvider = ({children}) => {
  const [loggedIn, setLoggedIn] = useState(() => {
    return typeof window === 'undefined' ? false : localStorage.getItem('loggedIn') === 'true';
  });

  useEffect(() => {
    const isUserLoggedIn = localStorage.getItem('loggedIn') === 'true';
    setLoggedIn(isUserLoggedIn);
  }, []);

  useEffect(() => {
    localStorage.setItem('loggedIn', loggedIn.toString());
  }, [loggedIn]);

  const login = () => {
    setLoggedIn(true);
  };
  const logout = () => {
    setLoggedIn(false);
  };

  return <AuthContext.Provider value={{loggedIn, login, logout}}>{children}</AuthContext.Provider>;
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```


{% endraw %}
