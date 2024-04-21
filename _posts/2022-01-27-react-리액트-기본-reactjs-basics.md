---
layout: post
date: 2022-01-27 17:14:00 +0900
title: '[React] 리액트 기본'
categories:
  - react
tags:
  - react
  - javascript
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [React \| Learn](https://react.dev/learn)
- [React \| Reference](https://react.dev/reference/react)
- [React \| Built-in React Hooks](https://react.dev/reference/react/hooks)
- [React \| Writing Markup with JSX](https://react.dev/learn/writing-markup-with-jsx)
- [React \| JavaScript in JSX with Curly Braces](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
- [비공식 튜토리얼#1](https://velopert.com/3613)

#### 테스트 환경 정보

- React 18


## 개요

리액트의 기초, 규칙, 문법 등을 정리한 글. 공식 문서의 한글화가 매우 잘 돼있다. 튜토리얼은 꼭 한 번 해볼 것.


## 설치

HTML 파일을 직접 작성할 건지, Node.js로 작업할 건지에 따라 갈린다.

### HTML에서 직접 불러오기

통칭 스크립트 태그 방식 혹은 CDN 방식. 빌드 과정을 생략하고 바로 리액트를 사용하려면 이렇게:

```html
<script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
```

두 개만 불러오면 된다. 리소스 주소는 [여기](https://reactjs.org/docs/cdn-links.html)서 확인할 수 있다.

CDN 방식에서 JSX 문법을 사용하고 싶다면, 아래처럼 [@babel/standalone](https://babeljs.io/docs/en/babel-standalone#installation)을 불러오고 스크립트 태그에 `type="text/bable"` 속성을 추가한다(이것을 *using in-browser Babel transformer*라고 부름):

```xml
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<div id="root"></div>
<script type="text/babel">
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const root = ReactDOM.createRoot(document.querySelector('#root'));
root.render(element);
</script>
```

하지만 이 방식은 잠재적 문제가 있으므로 가급적 멀리하고 아래의 SFC 방식으로 개발하도록 하자.

### Node.js로 빌드하기

이 쪽은 SFC(Single-File Components) 방식이라고 한다. SFC는 템플릿(HTML), 로직(JS), 스타일링(CSS)을 하나의 파일에 작성(Vue 메뉴얼에선 캡슐화라고 한다)할 수 있는 특수한 파일 포맷이라 하는데... (HTML도 마찬가지 아닌가?)

SFC 방식으로 웹 앱을 구축하려면, 자주 쓰는 패키지와 필수 파일 구조를 스캐폴딩 해주는 Create React App 패키지를 이용하는 것이 가장 편리하다. 해당 방법은 저 아래에 [CRA 항목](#heading-CRA-Create-React-App) 참고.

그게 아니면... React와 React DOM 패키지 정도만 설치해서 바로 개발이 가능하긴 한데, 이러면 JSX를 사용할 수 없고 웹팩이나 번들러를 별도로 설치해야 하는 등 번거로운 작업이 많으니 그냥 CRA, 그것도 아니면 Next.js, Vite를 쓰자.


## CRA, Create React App

[Create React App \| Getting Started](https://create-react-app.dev/docs/getting-started/)

```bash
npx create-react-app APP_NAME
# npm init react-app APP_NAME
# yarn dlx create-react-app APP_NAME
```

`APP_NAME`을 루트 경로로 하는 리액트 앱이 생성(scaffolding)되며, 해당 디렉터리로 이동해서 로컬 서버를 기동하거나 빌드, 테스트 등을 진행하면 된다.

### 초기 파일 구조

```
root
 ├─node_modules
 ├─public
 └─src
```

- `node_modules`: node 패키지 로컬 경로. 리액트 앱 개발에 필수적인 패키지들이 기본으로 설치되어 있다. react, react-dom, react-scripts, webpack, babel, eslin, postcss 등이 포함된다.
- `src`: 소스 코드가 위치하는 경로. 리액트 컴포넌트, 테스트 파일, CSS 파일과 기타 자바스크립트 모듈 등이 포함된다.
- `public`: 정적 파일들이 저장되는 경로다. 유일한 HTML이자 앱의 진입점 역할인 `index.html` 파일이 이 경로에 반드시 있어야 한다. 이 외에 파비콘 이미지, `robots.txt`, `manifest.json` 파일 등이 포함된다. 이 파일들은 빌드 시 `build` 디렉터리로 복사된다(특정 파일은 약간의 내용 수정이 있을 수 있음).

### CRA: react-scripts

react-scripts는 CRA에 포함된 스크립트 패키지다(CLI 명령어 패키지라고도 함). 로컬 서버 기동, 테스트, 배포 기능을 제공한다.

```bash
# 별칭: `npm start`. 
npm exec react-scripts start
```

`start`는 리액트 앱을 로컬에서 개발 모드로 기동한다. 개발 모드에서는 빌드 과정이 생략되며 소스 코드를 실시간으로 반영한다는 차이가 있다.

```bash
# 별칭: `npm run build`.
npm exec react-scripts build
```

`build`는 배포를 위한 빌드 명령어다. 빌드 결과는 `build` 디렉터리에 저장된다.

```bash
# 별칭: `npm test`.
npm exec react-scripts test
```

`test` 는 유닛 테스트를 실행한다. 이것도 실시간 감시 모드를 지원하여 소스 코드가 변경될 때마다 자동으로 테스트를 수행한다.

**TODO** 어떤 패키지를 사용하는지 + 어떤 파일이 테스트 대상인지

```bash
# 별칭: `npm run test`.
npm exec react-scripts eject
```

`eject`는 CRA로 구축된 리액트 앱에서 CRA 종속성을 제거하는 명령어다. CRA로 구축한 앱은 웹팩, 바벨, ESLint 등의 설정이 내부에 숨겨진 상태다. 이는 개발자가 복잡한 설정 없이 바로 코드 작성에 집중할 수 있기 위함인데, `eject`는 이 숨겨진 설정들을 직접 제어할 수 있도록 프로젝트 구조를 변경한다.

`eject`를 한 번 실행된 후에 다시 `CRA`로 되돌릴 방법은 버전관리 시스템에서 롤백하는 것 외엔 없다.

### CRA: CSS 적용하기

[Create React App \| Adding a Stylesheet](https://create-react-app.dev/docs/adding-a-stylesheet)

CRA에선 ESM의 `import`로 CSS를 가져올 수 있다:

```css
/*Button.css*/
.Button {
  padding: 20px;
}
```

```jsx
// Button.js
import {Component} from 'react';
import './Button.css'; // Tell webpack that Button.js uses these styles

class Button extends Component {
  render() {
    // You can use them as regular CSS styles
    return <div className="Button" />;
  }
}
```

원래는 자바스크립트 파일만 ESM으로 가져올 수 있지만, CRA에선 CSS 파일도 가져오는 게 가능하다. 대충 CRA가 웹팩에 CSS를 import 하라 알리면 웹팩이 CSS를 자바스크립트 모듈로 변환하는 식.

ℹ️ 이미지나 폰트 같은 파일도 import 구문으로 가져온다: [Create React App \| Adding Images, Fonts, and Files](https://create-react-app.dev/docs/adding-images-fonts-and-files)

### CRA: CSS Modules

[Create React App \| Adding a CSS Modules Stylesheet](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/)

CSS 모듈은 CSS 선택자의 유효범위를 모듈 단위로 제한하는 신박한 기능이다.

우선 `.module.css`로 끝나는 스타일 파일을 만들고:

```css
/*Button.module.css*/
.btn {
  color: white;
  background-color: tomato;
}
```

원하는 컴포넌트에서 모듈로 가져온 뒤, `className` 속성에 지정하면 된다. 이 때 객체 형태로 표기한다:

```jsx
// Button.js
import styles from './Button.module.css';

function Button({text}) {
  return (
    <button className={styles.btn}>{text}</button>
  );
}
```

결과는 아래와 같다:

```html
<button class="Button_btn__9-J4X">Continue</button>
```

이처럼 CSS 모듈은 `[filename]\_[classname]\_\_[hash]` 형식의 고유한 이름으로 클래스 이름을 자동 지정한다. 따라서 실제 `btn` 클래스 이름을 갖는 다른 요소와 충돌하지 않는 것이 특징이다.

### CRA: CSS Reset

[PostCSS Normalize](https://github.com/csstools/postcss-normalize)를 이용한 CSS 리셋 기능이다. CSS 파일에 다음 한 줄만 추가하면 된다:

```css
@import-normalize; /* bring in normalize.css styles */

/* rest of app styles */
```


## JSX

JavaScript XML 혹은 JavaScript eXtended. 페이스북이 리액트와 함께 만들었고 **컴파일이 필요한 언어**다. 스크립트 내에서 HTML 태그를 가독성 있게 작성하기 위해 사용한다.

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);

const image = <img src={user.avatarUrl} />;
```

여기서 괄호`()`는 세미콜론 자동 삽입을 예방하기 위한 것이며 필수는 아님.

### 이걸 브라우저가 읽을 수 있나?

당연히 그대로는 못 읽는다. JSX는 Babel에 의해 자바스크립트 파일로 컴파일된다. Babel 설치는 위의 [설치](#heading-설치) 항목을 보자

예를 들어 아래 코드는:

```jsx
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

```jsx
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
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<ShoppingList name="Mark" />);
```

아래와 같다:

```js
class ShoppingList extends React.Component {
  render() {
    return React.createElement(
      'div',
      {className: 'shopping-list'},
      React.createElement('h1', null, 'Shopping List for ', this.props.name),
      React.createElement(
        'ul',
        null,
        React.createElement('li', null, 'Instagram'),
        React.createElement('li', null, 'WhatsApp'),
        React.createElement('li', null, 'Oculus')
      )
    );
  }
}
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(React.createElement(ShoppingList, null));
```

컴파일 결과를 미리보고 싶으면 [여기를 눌러](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.6&spec=false&loose=false&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.16.9&externalPlugins=&assumptions=%7B%7D)볼 것.

### HTML 문법의 변화

우선 바디 없는 태그의 닫는 슬래시`/`를 생략할 수 없다:

```jsx
<input type="text" placeholder="Hours">
// Uncaught SyntaxError: /Inline Babel script: Unterminated JSX contents.
```

그리고 HTML 태그의 속성 이름에 새 규칙이 적용된다. 예를 들면:

- `class` -> `className`
- `colspan` -> `colSpan`
- `contenteditable` -> `contentEditable`
- `for` -> `htmlFor`
- `maxlength` -> `maxLength`
- `readonly` -> `readOnly`
- `tabindex` -> `tabIndex`
- `http-equiv` -> `httpEquiv`
- `classid` -> `classID`

`class`와 `for`, `http-equiv` 같은 몇 가지 예외가 있지만, 대체로 대소문자를 구분하는 카멜케이스 규칙이 적용되는 식이다. 이유는 기존 속성 이름이 자바스크립트의 키워드와 충돌하기 때문. 만약 이 규칙을 무시하면:

```jsx
<label for="minutes">Minutes</label>
// Warning: Invalid DOM property `for`. Did you mean `htmlFor`?
```

(개발모드일 때에 한해서) 이렇게 경고 로그가 발생한다.

### 자바스크립트 표현식 삽입

JSX에서 자바스크립트 표현식을 사용하려면 중괄호`{}`(Curly Braces)로 감싸면 된다:

```jsx
const element = <img src={user.avatarUrl}></img>;
```

```jsx
return (
  <h1>{name}'s To Do List</h1>
);
```

이중 중괄호`{{}}`가 감싼 코드를 볼 수 있는데, 첫 번째 중괄호는 자바스크립트 표현식을 알리는 중괄호이고, 두 번째 중괄호는 객체 리터럴의 중괄호다:

```jsx
const btn = <button style={{backgroundColor: 'green'}}>Click Me</button>;
```

참고로 JSX에서는 삽입된 모든 값을 렌더링 전에 이스케이프하기 때문에 **XSS 같은 명령어 주입 공격에서 안전**하다고 한다.

### JSX 요소는 단일 태그로 감싸야 함

그렇지 않은 표현은 문법 에러다:

```
const wrongSyntax = (
  <input type="text" />
  <button>push me</button>
);
// Parsing error: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?
```

아무 태그나 맨 바깥에 추가하면 되는데, 만약 어떤 태그도 추가하고 싶지 않을땐 더미 태그를 명시하는 방법이 있다:

```jsx
const correctSyntax = (
  <>
    <input type="text" />
    <button>push me</button>
  </>
);
```

### 프로퍼티 명명 규칙

React DOM은 HTML 어트리뷰트 이름 대신 camelCase 프로퍼티 명명 규칙을 사용한다. (예: `class` -> `className`, `tabindex` -> `tabIndex`)

### 중괄호의 생략

다음처럼 프로퍼티의 값이 string 리터럴인 경우:

```jsx
<div className={"abc"}><div>
```

중괄호는 생략해도 된다:

```jsx
<div className="abc"><div>
```

### 배열 표현

아래 둘은 같은 결과가 나온다:

```jsx
const UnorderedList = (
  <ul>
    {[
      <li key="1"></li>,
      <li key="2"></li>,
      <li key="3"></li>
    ]}
  </ul>
);

const UnorderedList2 = (
  <ul>
    <li key="1"></li>
    <li key="2"></li>
    <li key="3"></li>
  </ul>
);
```

그래서 [Array.prototype.map()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)을 활용하는 코드가 자주 보인다.

### 삼항 연산자 적용

```jsx
let doNotRender = true;
const conditional = (
  <div>
    {doNotRender ? (<></>) : (<div>hi</div>)}
  </div>
);
```


## 리액트의 주요 API

### 리액트 엘리먼트

리액트의 핵심(이지만 직접 호출할 일은 절대 없는) API인 `React.createElement()`는 객체를 생성하는데 이를 리액트 엘리먼트라고 하며 DOM을 구성할 때 사용된다.

리액트 엘리먼트 객체를 뜯어보면 대충 이렇게 생겼다:

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
  ref: null,
  type: function LikeButton(props) { ... }
}
```

### React.createElement

주어진 인자에 따라 새로운 React 엘리먼트를 생성하는 함수. JSX를 사용한다면 직접 호출할 일은 없다.

```
React.createElement(type)
React.createElement(type, props)
React.createElement(type, props, children)
React.createElement(type, props, children, children2, ...)
```

- `type`: 태그 이름 문자열 혹은 리액트 컴포넌트, 리액트 Fragment 중 하나
- `props`: 엘리먼트의 속성을 결정하는 object
- `children`: 원시 타입 값(보통은 문자열 혹은 숫자) 혹은 리액트 엘리먼트. 원시 타입를 할당하면 텍스트 노드가 된다.

만약 직접 호출한다면 아래처럼 사용한다:

```js
const element = React.createElement('h2', null, '대통령 선거', ' : ', '대통령 앉은거');
// <h2>대통령 선거 : 대통령 앉은거</h2>

const element2 = React.createElement('h2', {
  className: 'title',
  children: ['대통령 선거', ' : ', '대통령 앉은거']
});
// <h2 class="title"><span>대통령 선거</span><span> : </span><span>대통령 앉은거</span></h2>

const element3 = React.createElement('h2', {
  className: 'title',
  children: [
    React.createElement('span', {key: 1}, '대통령 선거'),
    React.createElement('span', {key: 2}, ' : '),
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

### ReactDOM.render()

```
ReactDOM.render(element, container)
ReactDOM.render(element, container, callback)
```

React 엘리먼트를 `container` 아래에 렌더링 하고 컴포넌트를 반환한다. 이미 렌더링 되었으면 업데이트한다. `callback`으로 전달된 함수는 렌더링 혹은 업데이트 후 실행된다.

```js
const rootElement = document.querySelector('#root');
const element = React.createElement('h2', null, '뿅뿅');
const ele = ReactDOM.render(element, rootElement);
console.log(Object.getPrototypeOf(ele)); // HTMLHeadingElement {...}
```

⚠️ 이 함수는 리액트 버전 18부터 지원하지 않는다. `ReactDOM.createRoot()`로 대체되었음. 위 코드는 아래처럼 바꿔야 한다:

```js
const element = React.createElement('h2', null, '뿅뿅');
const root = ReactDOM.createRoot(document.querySelector('#root'));
root.render(element);
console.log(element); // Object { "$$typeof": Symbol("react.element"), ... }
console.log(root); // Object { _internalRoot: {…} }
```

### ReactDOM.createRoot()

```
ReactDOM.createRoot(domNode)
ReactDOM.createRoot(domNode, options)
```

루트 DOM 노드에 리액트 컴포넌트 트리를 렌더링하는 새 함수. 리액트 버전 18부터 신규 추가되어 `ReactDOM.render()`를 대체한다.

[React \| createRoot()](https://react.dev/reference/react-dom/client/createRoot#createroot)

```jsx
import {createRoot} from 'react-dom/client';

const domNode = document.getElementById('root');
const root = createRoot(domNode);
root.render(<App />);
```

이 함수는 리액트의 동시성 모드 기능을 활성화한다. 동시성 모드를 활용하면 렌더링, 인테릭션 처리, 비동기 작업의 속도가 빨라진다고 한다.

\* 서버사이드 렌더링인 경우 `createRoot()` 대신 `hydrateRoot()`를 사용해야 함

### ReactDOM.hydrateRoot()

```
ReactDOM.hydrateRoot(domNode, reactNode)
ReactDOM.hydrateRoot(domNode, reactNode, options)
```

서버에서 렌더링된 마크업에 이벤트 핸들러 같은 자바스크립트 기능을 '부착' 시켜준다. `createRoot()`와 비슷하지만 이 함수는 SSR(Server-Side Rendering)일 때만 사용한다. 

ℹ️ *Hydration*은 클라이언트 측에서 자바스크립트를 실행하여 이미 존재하는 HTML 요소에 이벤트 리스너와 상태 등을 연결하는 과정을 말한다. 이를 통해 초기 HTML이 동적인 웹 애플리케이션으로 변환된다.

```js
import {hydrateRoot} from 'react-dom/client';

const domNode = document.getElementById('root');
const root = hydrateRoot(domNode, reactNode);
```

ℹ️ 예전에는 `ReactDOM.hydrate()`를 사용했지만 리액트 18부터 `hydrateRoot()`로 대체되었음


## 함수 컴포넌트 Function Components

리액트에서 [컴포넌트](https://react.dev/reference/react/Component)란 리액트 앱을 구성하는 최소 단위를 말한다. 컴포넌트의 최소 요구조건은 리액트 엘리먼트를 반환하는 것이다. 그래서 항상 뭔가를 반환한다.

함수 컴포넌트는 컴포넌트를 정의하는 가장 간단한 방법이며, 함수는 딱 하나의 인수 `props`를 전달 받는다. (간단하지 않은 방법으로는 클래스 문법으로 만드는 **클래스 컴포넌트**가 있는데, 이 글의 예시 중 `Newbie` 혹은 `ShoppingList`에 해당함.)

아래처럼 쓴다:

```jsx
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
const root = ReactDOM.createRoot(document.querySelector('#root'));
root.render(<Newbie/>);
```

\* 주의: 컴포넌트의 이름은 항상 대문자로 시작해야 함
\* 최신 버전에서(2024-03-03) [클래스 대신 함수로 컴포넌트를 정의하도록 권장](https://react.dev/reference/react/Component)하고 있다.


## 이벤트 핸들러 할당

요따구로 하면 됨:

```jsx
class Square extends React.Component {
  render() {
    return (
      <button
        onClick={() => {
          console.log('click');
        }}
      ></button>
    );
    // return React.createElement("button", {onClick: () => {console.log('click')}});
  }
}
```

`onclick`이 아니라 `onClick`인 것에 주의하자:

```jsx
<button type="button" onclick={() => setCounter(current => current + 1)}>Click Me</button>
// Warning: Invalid event handler property `onclick`. Did you mean `onClick`?
```


## state

state는 컴포넌트의 현재 상태를 나타내는 객체로, 컴포넌트의 데이터를 저장하고 관리하는 데 사용된다.

컴포넌트의 렌더링과 작동방식에 영향을 주기 때문에 반응형 상태값(reactive state)이라고도 한다(state 값의 변경은 컴포넌트의 리렌더링을 유발한다).

일반적으로 사용자 인터페이스에 영향을 주는 컴포넌트 내부의 변수나, 사용자 입력값을 state로 선언한다.

```jsx
const App = () => {
  const [counter, setCounter] = React.useState(0);
  const onClick = () => setCounter(current => current + 1);
  return (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button type="button" onClick={onClick}>Click Me</button>
    </div>
  );
};
```

한편, 16.8 버전 전에는 함수 컴포넌트에서 훅을 사용할 수 없었으며 아래처럼 클래스 컴포넌트를 선언하고 생성자에서 state를 초기화하는 방식으로 사용했었다:

```jsx
// 옛날 방식
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      helloText: 'Hello world!'
    }
  }

  render() {
    return (
      <div>{this.state.helloText}</div>
    );
  }
}
```

더 자세한 내용은 [훅만 따로 정리한 문서](/react/리액트-훅-reactjs-hooks/)를 볼 것.

### 폼 바인딩? 양방향 데이터 바인딩?

그런 거 없다. 리액트는 Vue의 `v-model`과 같은 사용자 입력값과 state를 자동으로 동기화하는 장치를 제공하지 않는다. 대신 우리가 직접 이벤트 핸들러를 작성해야 한다:

```jsx
function TextInput() {
  const [txt, setTxt] = React.useState(''); // 초기 state 설정

  const handleChange = (event) => {
    setTxt(event.target.value); // state 업데이트
  };

  return (
    <input type="text" value={txt} onChange={handleChange} />
  );
}
```


## props

요약하면 부모로부터 전달되는 읽기 전용 프로퍼티 집합이다.

리액트는 사용자 정의 컴포넌트의 태그 속성으로 전달된 값을 해당 컴포넌트에 단일 객체로 전달하는데 이 객체를 props라고 한다:

```jsx
function Header(props) {
  return (
    <h2>{props.helloText}</h2>
  );
}

function App() {
  return (
    <Header helloText='Hello world!'/>
  );
}
// <h2>Hello world!</h2> 출력
```

ℹ️ props의 변화는 자식 컴포넌트의 리렌더링을 유발한다. 단, 부모 컴포넌트에 state로 등록되어 있는 경우에만 그렇다. 만약 지역 변수나 `useRef`로 등록되었다면 값이 변해도 자식 컴포넌트의 리렌더링을 유발하지 않는다.

### props의 값은 변경할 수 없다

읽기 전용이므로 재할당은 에러를 유발한다:

```js
function Header(props) {
  props.helloText = 123; // Uncaught TypeError: "helloText" is read-only
  return (
    // ...
  );
}
```

### props.children

[React \| Passing JSX as children ](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children)

HTML에선 여는 태그와 닫는 태그가 존재하며 그 사이에 어떤 내용(content)을 작성할 수 있는 태그를 *바디 있는 태그* 혹은 *컨테이너 태그(container tag)*라고 부른다. 리액트에선 컨테이너 태그의 내용을 컴포넌트에 전달할 때 `props.children`을 사용한다(Vue의 `v-slot` 디렉티브와 비슷).

```jsx
// SomeComponent.js
function SomeComponent(props) {
  return <div>{props.children}</div>;
}
```

```jsx
// App.js
function App() {
  return (
    <SomeComponent>
      <h1>이 텍스트는 SomeComponent를 통해 렌더링 됨</h1>
    </SomeComponent>
  );
}
```

결과:

```html
<div>
  <h1>이 텍스트는 SomeComponent를 통해 렌더링 됨</h1>
</div>
```

### prop-types

- [npm \| prop-types](https://www.npmjs.com/package/prop-types)

props의 타입을 제한할 때 사용하는 패키지. CDN 방식이면 [여기](https://unpkg.com/prop-types/prop-types.js)에서 다운로드 할 것.

```jsx
import PropTypes from 'prop-types';

const Btn = (props) => {
  return (
    <button type={props.type} onClick={props.clickHandler}>
      {props.text}
    </button>
  );
};

Btn.propTypes = {
  text: PropTypes.string,
  clickHandler: PropTypes.func,
  type: PropTypes.string,
  backgroundColor: PropTypes.string,
  fontSize: PropTypes.number
};

const App = () => {
  return (
    <div>
      <Btn text='some-text' fontSize={'18'}></Btn>
    </div>
  );
};
```

위 코드에서 `Btn` 컴포넌트의 `fontSize`를 `number`로 제한했다. 하지만 실제 넘겨진 값은 `'18'`이므로 아래와 같은 경고창이 발생하는 식이다:

```
Warning: Failed prop type: Invalid prop `fontSize` of type `string` supplied to `Btn`, expected `number`.
```

그리고 필수값으로 지정하는 방법도 제공한다:

```jsx
Btn.propTypes = {
  type: PropTypes.string.isRequired,
};
```

`.isRequired`로 `type`를 필수값으로 지정했다. 여기서 만약 `type`을 제공하지 않으면:

```
Warning: Failed prop type: The prop `type` is marked as required in `Btn`, but its value is `undefined`.
```

이렇게 된다.


## React.memo()

`memo()`는 리액트의 고차 컴포넌트(Higher-Order Component)로 컴포넌트의 props가 변경되지 않은 경우 리렌더링을 건너뛰라 지시할 때 사용한다.

ℹ️ `memo()`는 props의 변환를 얕은 비교(shallow comparison)를 통해 감지한다. 객체의 가장 상위 레벨에 있는 프로퍼티만 비교한다는 뜻이다.

```
React.memo(SomeComponent)
React.memo(SomeComponent, arePropsEqual)

function arePropsEqual(oldProps, newProps)
```

- `SomeComponent`: 메모(memoize)하려는 컴포넌트. 함수 컴포넌트, forwardRef 컴포넌트 등 모든 리액트 컴포넌트를 할당할 수 있다.
- `arePropsEqual`: 이전 props와 새로운 props를 인수로 받는 함수. `true`는 같음을, `false`는 다름을 의미한다. props의 특정 프로퍼티만 감지할 때 사용한다.

반환 값은 `SomeComponent`를 기반으로 생성된 메모화(memoized)된 컴포넌트다.

```jsx
const MyComponent = React.memo(props => {
  return <div>{props.value}</div>;
});
```

`arePropsEqual`는 특정 props가 변화했을 때만 리렌더링을 유발하고 싶을 때 사용되는 `memo()`의 두 번째 인자다. 요렇게 쓴다:

```jsx
const areEqual = (prevProps, nextProps) => {
  // true를 반환하면 업데이트를 트리거하지 않음
  // false를 반환하면 업데이트를 트리거함
  return prevProps.value === nextProps.value;
};

// 이 컴포넌트는 props 중 value가 변화할 때만 리렌더링함
const MyComponent = React.memo(props => {
  return <div>{props.value}</div>;
}, areEqual);
```


## 라우터 React Router

[React Router](https://reactrouter.com/)

모던 프론트엔드 개발에서 라우팅이란 보통 하나의 HTML로 구성된 페이지에서 다른 URL로 이동하지 않고 사용자의 상호 작용에 따라 동적으로 화면 내용을 업데이트하는 것을 말한다.

리액트에서 라우팅을 구현하려면 별도의 react-router 패키지 설치가 필요하다.

```bash
npm install react-router-dom
```

react-router-dom 패키지는 react-router의 확장이며 react-router를 내부에 포함하고 있으니 이것만 설치하면 된다.

사용방법은 버전에 따라 다른데, 5.x 버전의 경우 아래와 같고:

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

```jsx
// App.js
import {
  useState,
  useEffect
} from 'react';
import {
  BrowserRouter as Router,
  Switch,
  Route,
} from 'react-router-dom';

import Button from './component/Button';
import Home from './route/Home';
import Detail from './route/Detail';

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/movie">
          <Detail />
        </Route>
        <Route path="/">
          <Home />
        </Route>
      </Switch>
    </Router>
  );
}

export default App;
```

최신(2024-03-31 기준) 버전인 6.x에선 많이 달라졌다:

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import {createBrowserRouter, RouterProvider} from 'react-router-dom';

import Root from './routes/root';
import Hello2 from './routes/hello2';
import Hello1 from './routes/hello1';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      {
        path: 'hello1',
        element: <Hello1 />
      },
      {
        path: 'hello2',
        element: <Hello2 />
      }
    ]
  }
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

```jsx
// routes/root.jsx
import {Link, Outlet} from 'react-router-dom';

export default function Root() {
  return (
    <>
      <div id="sidebar">
        <h1>React Router Example</h1>
        <nav>
          <ul>
            <li>
              <Link to={`/`}>home</Link>
            </li>
            <li>
              <Link to={`/hello1`}>hello 1</Link>
            </li>
            <li>
              <Link to={`/hello2`}>hello 2</Link>
            </li>
          </ul>
        </nav>
      </div>
      <div id="detail">
        <Outlet />
      </div>
    </>
  );
}
```

자세한 내용은 [도움말 참고](https://reactrouter.com/en/main/start/tutorial).
