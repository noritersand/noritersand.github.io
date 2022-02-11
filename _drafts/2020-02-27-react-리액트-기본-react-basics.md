---
layout: post
date: 2020-02-27 17:14:00 +0900
title: '[React] 리액트 기본 React Basics'
categories:
  - react
tags:
  - react
  - javascript
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[React\] 튜토리얼](https://ko.reactjs.org/tutorial/tutorial.html)
- [\[React\] 시작하기](https://ko.reactjs.org/docs/getting-started.html)
- [\[React\] 성능 최적화](https://ko.reactjs.org/docs/optimizing-performance.html)
- [https://github.com/facebook/react](https://github.com/facebook/react)
- [비공식 튜토리얼#1](https://velopert.com/3613)

#### 버전 정보

- React 17

## 개요

리액트의 기초, 규칙, 문법 등을 정리한 글.

공식 문서의 한글화가 매우 잘 돼있다. 튜토리얼은 꼭 한 번 해볼 것.

## React 엘리먼트

`React.createElement()`는 객체를 생성하는데 이를 리액트 엘리먼트라고 하며 DOM을 구성할 때 사용된다.

객체는 대충 이렇게 생김:

```js
{
  "$$typeof": Symbol(react.element),
  _owner: null,
  _self: null,
  _source: null,
  _store: {…},
  validated: false,
  key: null,
  props: Object {  },
  ​ref: null,
  type: function LikeButton(props)​ { ... }
}
```

## JSX

JavaScript XML 혹은 JavaScript eXtended로 추정. 페이스북이 react.js와 함께 만든것으로 보임. **컴파일이 필요한 언어**이며 스크립트 내에서 (가독성을 위해)HTML 태그를 사용하려는 목적으로 사용된다.

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);

const image = <img src={user.avatarUrl} />;
```

여기서 괄호`()`는 세미콜론 자동 삽입을 예방하기 위한 것이며 필수는 아님.

### 태그 속성 정의

```js
const element = <div tabIndex="0"></div>;
```

### 자바스크립트 표현식 삽입

```js
const element = <img src={user.avatarUrl}></img>;
```

JSX에서는 삽입된 모든 값을 렌더링 전에 이스케이프하기 때문에 **XSS 같은 명령어 주입 공격에서 안전**하다고 한다.

### 프로퍼티 명명 규칙

React DOM은 HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용한다. (e.g. `class` => `className`, `tabindex` => `tabIndex`)

### 이걸 브라우저가 읽을 수 있나?

당연히 그대로는 못 읽는다. JSX는 Babel에 의해 js로 컴파일된다. Babel 설치는 아래의 [설치](#heading-설치) 항목을 보자

예를 들어 아래 코드는:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

이렇게 컴파일된다:

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

하나 더 예를 들면:

```js
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
ReactDOM.render(
  <ShoppingList />,
  document.getElementById('root')
);
```

아래와 같다:

```js
class ShoppingList extends React.Component {
  render() {
    return React.createElement("div", { className: "shopping-list" },
        React.createElement("h1", null, "Shopping List for ", this.props.name),
        React.createElement("ul", null,
            React.createElement("li", null, "Instagram"),
            React.createElement("li", null, "WhatsApp"),
            React.createElement("li", null, "Oculus")
        )
    );
  }
}
ReactDOM.render(
  React.createElement(ShoppingList, null),
  document.getElementById('root')
);
```

컴파일 결과를 미리보고 싶으면 [여기를 눌러](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.6&spec=false&loose=false&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.16.9&externalPlugins=&assumptions=%7B%7D)볼 것.

## 설치

HTML 파일을 직접 작성할 건지, Node.js로 작업할 건지에 따라 갈린다.

### HTML

리액트는 라이브러리라서 js 파일을 불러오기만 하면 된다. 보통은 이렇게:

```js
<script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
```

두 개를 끌어다 씀. CDN 링크는 [여기를 클릭](https://reactjs.org/docs/cdn-links.html).

JSX 문법을 사용하려면 아래처럼 [@babel/standalone](https://babeljs.io/docs/en/babel-standalone#installation)을 불러오고 내 스크립트에 `type="text/bable"` 속성을 추가한다:

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<div id="root"></div>
<script type="text/babel">
  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );
  ReactDOM.render(element, document.querySelector('#root'));
</script>
```

### Node.js

[Create React App](https://create-react-app.dev/docs/getting-started/) 패키지를 npx로 설치한다:

```bash
npx create-react-app APP_NAME
```

그리고 `APP_NAME` 디렉터리에서:

```bash
npm start
```

하면 샘플 페이지가 있는 웹 서버가 기동된다. HTML 없이 그냥 되는거 아니다. `public/index.html`을 확인할 것.

바벨은 리액트 패키지에 포함되어 있어서 별도 설치 불필요하며, 이미 만들어둔 앱이면 `npm start` 전에 모듈 설치 필요할 수 있음:

```bash
npm install
# 혹은
yarn install
```

## 주요 API

### [React.createElement](https://ko.reactjs.org/docs/react-api.html#createelement)

```
React.createElement(
  type[, props][, children][, children2][, ...]
)
```

- `type`: 태그 이름 문자열 혹은 리액트 컴포넌트, 리액트 Fragment 중 하나
- `props`: 엘리먼트의 속성을 결정하는 object
- `children`: 원시 타입 값(보통은 문자열 혹은 숫자) 혹은 리액트 엘리먼트. 원시 타입를 할당하면 텍스트 노드가 된다.

주어진 인자에 따라 새로운 React 엘리먼트를 생성하는 메서드. JSX를 사용한다면 직접 호출할 일은 거의 없다.

사용하는 방법은 여러가지다.

```js
const element = React.createElement('h2', null, '대통령 선거', ' : ', '대통령 앉은거');
// <h2>대통령 선거 : 대통령 앉은거</h2>

const element2 = React.createElement('h2', {
  className: 'title',
  children: [
    '대통령 선거', ' : ', '대통령 앉은거'
  ]
});
// <h2 class="title"><span>대통령 선거</span><span> : </span><span>대통령 앉은거</span></h2>

const element3 = React.createElement('h2', {
  className: 'title',
  children: [
    React.createElement('span', { key: 1 }, '대통령 선거'),
    React.createElement('span', { key: 2 }, ' : '),
    React.createElement('span', null, '대통령 앉은거')
  ]
});
// <h2 class="title"><span>대통령 선거</span><span> : </span><span>대통령 앉은거</span></h2>
```

#### 노드 리스트의 Key

위 예시에서 `element3`는 `props.children`으로 하위 노드를 할당하는 식인데, `props` 자리에 세 번째 span 태그처럼 `{ key: somevalue }` 대신 null을 넘기면 `Warning: Each child in a list should have a unique "key" prop.`라는 경고 메시지가 발생한다.

> 리스트를 렌더링할 때 React는 렌더링하는 리스트 아이템에 대한 정보를 저장합니다. 리스트를 업데이트 할 때 React는 무엇이 변했는 지 결정해야 합니다. 리스트의 아이템들은 추가, 제거, 재배열, 업데이트 될 수 있습니다.
> ... 현재 리스트가 이전 리스트에 존재했던 키를 가지고 있지 않다면 React는 그 키를 가진 컴포넌트를 제거합니다. 두 키가 일치한다면 해당 구성요소는 이동합니다. 키는 각 컴포넌트를 구별할 수 있도록 하여 React에게 다시 렌더링할 때 state를 유지할 수 있게 합니다. 컴포넌트의 키가 변한다면 컴포넌트는 제거되고 새로운 state와 함께 다시 생성됩니다.
>
> [https://ko.reactjs.org/tutorial/tutorial.html#picking-a-key](https://ko.reactjs.org/tutorial/tutorial.html#picking-a-key)

대충 요약하면 렌더링 최적화에 필요한 프로퍼티다:

```xml
<li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
```

전역에 걸쳐 유일한 값일 필요는 없으며 컴포넌트 내에서만 유일하면 된다고 함.

그리고 `key`가 `props`에 속하는 것처럼 보이지만 `this.props.key`로 참조할 수 없다고 한다. 일종의 숨겨진 프로퍼티로 작동하는 모양.

### [ReactDOM.render()](https://ko.reactjs.org/docs/react-dom.html#render)

```
ReactDOM.render(element, container[, callback])
```

React 엘리먼트를 `container` 아래에 렌더링 하고 컴포넌트를 반환한다. 이미 렌더링 되었으면 업데이트한다. `callback`으로 전달된 함수는 렌더링 혹은 업데이트 후 실행된다.

```js
const rootElement = document.querySelector('#root');
const element = React.createElement('h2', null, '뿅뿅');
const ele = ReactDOM.render(element, rootElement);
console.log(Object.getPrototypeOf(ele)); // HTMLHeadingElement {...}
```

### [ReactDOM.hydrate()](https://ko.reactjs.org/docs/react-dom.html#hydrate)

```
ReactDOM.hydrate(element, container[, callback])
```

> render()와 동일하지만 HTML 컨텐츠가 ReactDOMServer로 렌더링 된 컨테이너에 이벤트를 보충하기 위해 사용됩니다. React는 기존 마크업에 이벤트 리스너를 연결합니다.

TODO 뭔 소린지 모르겠음. SSR 페이지는 이거 쓰라고 검색 결과에 나오긴 하는데...

## 함수 컴포넌트 Function Components

[리액트에서 컴포넌트](https://reactjs.org/docs/components-and-props.html)란 리액트 앱을 구성하는 최소 단위를 말한다. 컴포넌트의 최소 요구조건은 리액트 엘리먼트를 반환하는 것이다. 그래서 항상 `return` 문을 포함한다.

함수 컴포넌트는 컴포넌트를 정의하는 가장 간단한 방법이며, 함수는 딱 하나의 인수 `props`를 전달 받는다. (간단하지 않은 방법으로는 클래스 문법으로 만드는 클래스 컴포넌트가 있는데, 이 글의 예시 중 `Newbie` 혹은 `ShoppingList`에 해당함.)

아래처럼 쓴다:

```js
function Noop(props) {
  return (
    <div>{props.foo}</div>
  );
}
class Newbie extends React.Component {
  render() {
    return (
      <Noop foo="bar"/>
    );
  }
}
ReactDOM.render(<Newbie/>, document.querySelector('#root'));

// 결과:
// <div id="root">
//   <div>bar</div>
// </div>
```

## 이벤트 핸들러 할당

그냥 요따구로 하면 됨:

```js
class Square extends React.Component {
  render() {
    return (
      <button
          className="square"
          onClick={function() {
            console.log('click');
          }}
      >
      </button>
    );
    // return React.createElement("button", { className: "square", onClick: function () { console.log('click'); } });
  }
}
```

## State

[컴포넌트 State](https://ko.reactjs.org/docs/faq-state.html)

state는 (생성자에서 초기화하는 것은 제외하면) 직접 값을 재할당하는게 아니라 `setState()` 통해서 리액트에 해당 컴포넌트와 관련 컴포넌트들이 다시 렌더링해야 한다고 알리는 방식을 쓴다:

```js
state = {
  someFlag: false
}

this.state.someFlag = true; // X
this.setState({ someFlag: true }); // O
```

리액트는 컴포넌트가 다시 렌더링을 할 때까지 state의 값을 갱신하지 않는다. [링크: this.state는 왜 즉시 갱신되지 않는가?](https://github.com/facebook/react/issues/11527#issuecomment-360199710)  

그래서 `setState()` 내부 혹은 바로 다음 줄에서 state를 읽는 코드는 문제를 일으킬 수 있다:

```js
// 이런거나
this.setState({
  count: ++this.state.clickCount // X
});

// 이런거
this.setState({ count: 999 });
console.log(this.state.count); // X
```

대신 다음처럼 updater 함수를 전달하는 방법이 권장된다:

```js
this.setState((state) => {
  return {
    count: state.count++
  }
});
```

## Props

[Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)

부모로부터 전달되는 읽기 전용 프로퍼티 집합.

TODO
