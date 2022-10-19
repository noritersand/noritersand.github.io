---
layout: post
date: 2021-12-20 23:44:40 +0900
title: '[JavaScript] 모듈, 내보내기와 가져오기'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - module
  - export
  - import
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [\[MDN\] import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
- [\[MDN\] export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

#### 브라우저 호환

- ES2015에서 최초 정의
- IE는 사용 불가
- 안드로이드 웹뷰, 사파리, 파이어폭스, 오페라에서 일부 지원 안되는 기능 있음


## 개요

ES2015의 새로운 기능인 module(이하 모듈)에 대한 간단한 설명 글. 함수, 객체, 원시 값을 내보내거나 가져올 때 사용한다. RequireJS, React 같은 웹 프레임웤이나 Node.js 사용자라면 이미 익숙할 것이다.

모듈은 export와 import 구문으로 구현하며, 내보내거나 가져오는 모듈은 무조건 엄격 모드로 작동한다는 특징이 있다. 그리고 라이브러리(기존 방식을 말함)를 사용하는 것보다 더 효율적이라는데 그건 잘 모르겠고 ㅎㅎ...

모듈 기능을 테스트하려면 웹 서버가 필요하다. 브라우저로 HTML 파일을 직접 열면 교차 출처 차단으로 정상 작동하지 않는다.

```js
// 파폭
교차 출처 요청 차단: 동일 출처 정책으로 인해 file:///C:/dev/git/mdn-js-examples/modules/basic-modules/main.js에 있는 원격 리소스를 차단하였습니다. (원인: http가 아닌 CORS 요청).
이 문서에서 모듈 원본 URI가 허용되지 않음: “file:///C:/dev/git/mdn-js-examples/modules/basic-modules/main.js”.

// 크롬
Access to script at 'file:///C:/dev/git/mdn-js-examples/modules/basic-modules/main.js' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, chrome-untrusted, https.
index.html:11 GET file:///C:/dev/git/mdn-js-examples/modules/basic-modules/main.js net::ERR_FAILED
```

이 글에서 import의 우리말 표기는 '불러오기'와 '가져오기' 중 검색결과가 약간 더 많은 '가져오기'로 적는다.


## 기본 규칙

### 모듈 선언

스크립트를 모듈로 선언하려면 HTML 페이지에 적용할 때 `type="module"`을 추가해야 한다:

```html
<script type="module" src="main.js"></script>
```

이제 `main.js`는 모듈로 인식되어 작동할 것이다.

import와 export 구문은 모듈 내에서만 사용할 수 있다. 모듈이 아닌 경우 아래와 같은 오류 메시지가 나타난다:

```js
// 파폭
Uncaught SyntaxError: import declarations may only appear at top level of a module

// 크롬
Uncaught SyntaxError: Cannot use import statement outside a module
```

MDN의 가이드를 보면 모듈이 아닌 것은 일반 스크립트(standard scripts) 혹은 라이브러리라고 부른다.

### 내보내기

```js
// 선언하며 내보내기
export const name = 'Waldo';
export function doSomething() {}

// 선언 후 내보내기
const ZERO = 0;
export { ZERO };
```

내보내기가 가능한 대상은 functions, `var`, `let`, `const`, class이며 최상위 유효 범위에 있어야 한다. 그러니께 함수 안에서 내보내기는 불가능.

### 가져오기

```js
import { name } from './MY_MODULE.js';
```

import 구문은 가져올 대상 뒤에 `from` 키워드로 모듈 파일의 경로를 명시한다. 위 예시는 같은 경로에 있는 `MY_MODULE.js` 파일에서 `name`을 가져오라는 의미다.

모듈로 선언된 경우 인터널(=임베드) 스크립트에서도 `import`를 쓸 수 있다. 예를 들어 아래 코드도 잘 돌아간다:

```html
<script type="module">
import { create, createReportList } from './modules/canvas.js';

var myCanvas = create('myCanvas', document.body, 480, 320);
var reportList = createReportList(myCanvas.id);
</script>
```

주의: **import 된 모듈의 유효범위는 전역이 아니다**([module features are imported into the scope of a single script](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#other_differences_between_modules_and_standard_scripts)). 따라서 콘솔에서 호출할 수 없다.


## 하나씩 내보내고 가져오기

```js
// module1.js
export let a = 1;

const b = 2;
export { b };
```

```js
// main.js
import { a } from './module1.js';
import { b } from './module1.js';

console.log(a); // 1

// const는 const로 가져오므로 재할당 불가
b = 3; // Uncaught TypeError: Assignment to constant variable.
```


## 목록으로 내보내고 가져오기

```js
// module1.js
export const a = 1, b = '2';
export let obj = { a, b };
```

```js
// main.js
import { a, b, obj } from './module1.js';

console.log(a); // 1
console.log(b); // "2"
console.log(JSON.stringify(obj)); // {"a":1,"b":"2"}
```


## 내보내면서 이름 바꾸기

```js
// module1.js
var c = 'cfoot';
var d = 'dorat?';

export { c as foot, d as doratman };
```

```js
// main.js
import { foot, doratman } from './module1.js';

console.log(foot); // cfoot
console.log(doratman); // dorat?
```


## 가져오면서 이름 바꾸기

```js
// module1.js
var c = 'cfoot';
var d = 'dorat?';

export { c, d };
```

```js
// main.js
import { c as foot, d as doratman } from './module1.js';

console.log(foot); // cfoot
console.log(doratman); // dorat?
```


## 클래스 내보내기

```js
// module1.js
export class Newbie {
  levelUp() {
    console.log('I feel stronger.');
  }
}
```

```js
// main.js
import { Newbie } from './module1.js';

var noob = new Newbie();
noob.levelUp(); // I feel stronger.
```


## 구조 분해 할당식으로 내보내기

참고: [Assigning to new variables names and providing default values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#assigning_to_new_variables_names_and_providing_default_values)

```js
// module1.js
export const { str1, str2: bar } = { str1: 'abc', str2: 'def'};
```

```js
// main.js
import { str1, bar } from './module1.js';

console.log(str1); // "abc"
console.log(bar); // "def"
```


## 가져온 모듈을 다시 내보내기

계층 구조를 갖는 여러 모듈이 있을 때 서브모듈의 기능을 가져와 다시 내보내는 방법이다. `export` 뒤에 `from`이 붙으면 서브모듈이 있다고 보면 된다.

```js
// module2.js
export const PI = 3.14;
export const ZERO = 0;
```

```js
// module1.js
import { PI } from './module2.js';
export { PI };

// 위 코드를 한 줄로 쓰면 이렇게 됨
export { ZERO } from './module2.js';
```

```js
// main.js
import { PI } from './module1.js';
console.log(PI); // 3.14

import { ZERO } from './module1.js';
console.log(ZERO); // 0
```


## 객체로 가져오기

모듈의 기능(MDN에서 feature라고 표현함)을 모듈 객체 안으로 가져오는 방법.

```js
// module1.js
export const ONE = 1;
export const GOLDEN_RATIO = 1.618;
export const KAPREKA_NNUMBER = '495, 6174';
```

```js
// main.js
import * as math from './module1.js';

console.log(math.ONE); // 1
console.log(math.GOLDEN_RATIO); // 1.618
console.log(math.KAPREKA_NNUMBER); // "495, 6174"
```


## default export, import 기본값으로 내보내고 가져오기

주의: [일부 브라우저에서 제한되는 기능](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export#browser_compatibility)임.

**모듈 하나 당 딱 하나만 배정된 기본값**을 이용해 내보내는 방법. 경우에 따라 내보내는 기능의 이름을 생략할 수 있다.

```js
// module2.js
// 익명으로 선언하고 기본값으로 내보내기
export default function() {
  console.log('I\'m waldo.');
}
```

```js
// module1.js
// 선언 후 기본값으로 내보내기
function fn() {
  console.log('Oh hello there!');
}
export { fn as default }; // 함수 fn()을 default로 내보내기
```

반면 가져오는 모듈에서는 반드시 이름을 지정해야 한다:

```js
// main.js
import { default as hello } from './module1.js';
hello(); // Oh hello there!

import yourName from './module2.js'; // 중괄호와 default는 생략 가능
yourName(); // I'm waldo.
```

이름을 지정할 땐 중괄호와 `default as`를 생략할 수 있다. 중괄호를 생략하면 무조건 기본값을 참조한다고 간주된다. 요 특성을 잘 모르면 기본값 사용이 아닌 가져오기에서 자꾸 문법 에러가 발생할 것이다.


## Dynamic module loading 동적 모듈 로딩

필요할 때만 모듈을 동적으로 가져오는 기능이다. 스크립트 파일을 원하는 시점에 로딩하기 때문에 성능 이점이 있다. 사용하려면 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)에 대해 대충이라도 아는게 좋음.

`import()` 함수는 Promise 객체를 반환하는데, 해당 객체를 통해 모듈에서 내보낸 기능에 접근한다. 아래 예시를 보자:

```js
// module1.js
export const message = 'wassssssssssssup';
// 내보내는 모듈에선 특이한 게 없다.
```

```js
// main.js
var btn = document.querySelector('button#btn');

btn.addEventListener('click', () => {
  import('./module1.js').then((module) => {
    alert(module.message); // 경고창 "wassssssssssssup" 표시
  });
});
```

생략했지만 HTML 페이지에 버튼 태그가 하나 있는 상태고, 이 버튼을 누르면 `module1.js`의 `message`를 가져와서 경고창으로 보여주는 코드다. 트래픽을 확인해보면 버튼을 눌렀을 때 `module1.js` 파일을 로딩한다는 걸 확인할 수 있다.


## 언급하지 않은 조합

```js
export { name1 as default, ... };
export * from ...; // does not set the default export
export * as name1 from ...;
export { name1, name2, ..., nameN } from ...;
export { import1 as name1, import2 as name2, ..., nameN } from ...;
export { default } from ...;

import { export1 , export2 as alias2 , [...] } from "module-name";
import myDefault, { export1 [ , [...] ] } from "module-name";
import myDefault, * as name from "module-name";
import "module-name"; // 변수 바인딩 없이 스크립트를 실행만 할 때 사용한다.
```

끝. 🥱
