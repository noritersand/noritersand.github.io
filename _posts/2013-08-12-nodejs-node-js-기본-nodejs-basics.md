---
layout: post
date: 2013-08-12 16:36:00 +0900
title: '[Node.js] Node.js 기본'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://nodejs.org/en/docs/](https://nodejs.org/en/docs/)
- [http://www.nodebeginner.org/index-kr.html](http://www.nodebeginner.org/index-kr.html)

![](/images/node-js-1.png)
> node에서는 모든게 병렬로 수행된다. 당신 code만 빼고


## 시작하기

### 설치

아래 링크에서 OS에 맞는 파일을 받아 설치한다.
[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

안될 경우 `NODE_PATH = C:\Users\noriter\AppData\Roaming\npm\node_modules` 패스 추가.

### 구동

```bash
node 파일명
```

node 명령어는 단독으로 사용했을 때 REFL이라고 하는 node.js shell로 진입한다. 위처럼 파일명과 같이 사용하면 해당 파일을 node.js로 실행하라는 의미이다.

### sync/async

쉽게 표현해서, 동기 방식인 blocking I/O가 아래처럼 돌아간다면:

```js
puts("이름을 입력하세요");
var name = gets();
puts("당신의 이름은" + name + "입니다.");
```

비동기 방식인 non-blocking I/O는 아래와 같다:

```js
puts("이름을 입력하세요");
gets(function (name) {
  puts("당신의 이름은" + name + "입니다.");
});
```

### 이벤트 루프란?

프로세스의 처리과정을 이벤트의 순환으로 구현한 것, 혹은 이벤트를 처리하는 스택구조 자체를 이벤트 루프라 함.

### 노드 표준 코딩 관례

- 들여쓰기: 공백 2칸
- 문장종결: 문장의 끝은 자바스크립트의 관례대로 세미콜론(;)을 사용함
- 문자열: 문자열은 큰따옴표
("") 대신 작은따옴표
('')를 사용한다.
- 블록을 시작하는 중괄호는 시작하는 문장과 같은 라인에 작성한다.
- 변수와 프로퍼티: 소문자로 시작하는 카멜케이스를 사용한다.
- 클래스: 대문자로 시작하는 카멜케이스를 사용한다.
- 동등 비교: == 대신 ===를 사용한다.
- 콜백 함수: 콜백 함수에서 첫 파라미터(혹은 인자)는 노드 코어의 콜백 함수처럼 에러 파라미터로 사용한다.
- [nodejs style guide(eng).pdf](/attachments/nodejs-style-guide-eng.pdf)
- [nodejs style guide(kor).pdf](/attachments/nodejs-style-guide-kor.pdf)


## exports/require

노드에서 하나의 자바스크립트 파일은 하나의 모듈이 된다. 기본적으로 노드는 각 자바스크립트 파일을 익명함수로 감싸 외부에서 접근할 수 없게 만드는데 이것을 모듈화라고 하며 모듈간 참조와 호출을 위해 글로벌 객체 module의 exports/require를 사용한다.

### module.exports

모듈로 내보내기.

```js
//Syntex 1: module.exports.내보낼함수명 = 함수명
function drawCircle() {
  ...
}
exports.drawCircle = drawCircle;

//Syntex 2: module.exports = 함수리터럴 혹은 익명함수
exports.fibonacchi = function (num) {
  return -1;
};

//Syntex 3: 변수 = module.exports = 함수
var show = exports.show = function () {
  console.log('hi');
}
```

`module.exports`에서 `module`은 전역 객체이기 때문에 생략할 수 있다. **단, `exports`의 프로퍼티가 아닌 `exports`에 직접 할당할 때는 제외**.

### require

모듈 가져오기.

```
require(경로/파일명);
```

경로는 문자열`' '`로 표시하며 경로를 생략할 경우 기본모듈이나 확장모듈(node.js가 기본적으로 제공하는 모듈 혹은 npm으로 설치한 모듈을 말하며 이 둘을 *native module*이라 한다.)을 가져온다.

같은 경로내에 가져올 js 파일이 있다면 경로는 `./`가 되고 한 단계 상위의 폴더라면 경로는 `../`가 된다.

```js
var userModule = require('./user_module');
```

`require()`로 호출된 파일은 노드 애플리케이션 내에 캐싱된다. 다시 말해 같은 파일을 여러번 호출해도 최초로 생성되었던 객체가 반복적으로 반환된다. 별도의 인스턴스가 필요하다면 함수를 따로 호출하거나, 함수 자체를 내보내 `new`로 인스턴스를 생성해서 사용해야 함.

#### 작성 예시 \#1

```js
function printA() {};
var PI = 3.14;

exports.printA = printA;
exports.PI = PI;

// 위에서 내보낸 모듈은 다음처럼 가져온다
var userModule = require('js를_제외한_파일_경로');
userModule.printA();
console.log(userModule.PI);
```

#### 작성 예시 \#2

```js
// exports-test.js
module.exports = { // 이렇게 내보낼 땐 module을 생략할 수 없음
  connectString: '10.20.30.40:1234/QADB',
  user: 'fixalot',
  password: '1234abcd!'
}
```

```js
// run-me.js
const dbinfo = require('./exports-test.js');
console.log('dbinfo.connectString:', dbinfo.connectString);
console.log('dbinfo.user:', dbinfo.user);
console.log('dbinfo.password:', dbinfo.password);
```

```bash
PS> node .\run-me.js
dbinfo.connectString: 10.20.30.40:1234/QADB
dbinfo.user: fixalot
dbinfo.password: 1234abcd!
```

### require로 코드 줄이기

자주 사용하는 메서드가 있다면 `require`로 해당 메서드를 별도의 변수에 담아 코드를 줄이는 방법이 있다:

```js
const log = require('console').log;
```

여기에 ES2015의 단축 표기법을 적용하면:

```js
const {log} = require('console');
```

이렇게 된다.

그런데 사실 `console`은 어차피 전역 객체이므로 그냥 아래처럼 쓰면 됨:

```js
const {log} = console;
```
