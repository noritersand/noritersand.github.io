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

#### 참고한 문서

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
gets(function(name) {
  puts("당신의 이름은" + name + "입니다.");
});
```

### 이벤트 루프란?

프로세스의 처리과정을 이벤트의 순환으로 구현한 것, 혹은 이벤트를 처리하는 스택구조 자체를 이벤트루프라 함.

### 노드 표준 코딩 관례

- 들여쓰기: 공백 2칸
- 문장종결: 문장의 끝은 자바스크립트의 관례대로 세미콜론(;)을 사용함
- 문자열: 문자열은 쌍따옴표("") 대신 홑따옴표('')를 사용한다.
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
exports.fibonacchi = function(num) {
  return -1;
};

//Syntex 3: 변수 = module.exports = 함수
var show = exports.show = function() {
  console.log('hi');
}
```

`module.exports`에서 `module`은 전역 객체이기 때문에 생략할 수 있다. **단, `exports`의 프로퍼티가 아닌 `exports`에 직접 할당할 때는 제외**.

### require()

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
function printA(){};
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
const { log } = require('console');
```

이렇게 된다.

그런데 사실 `console`은 어차피 전역 객체이므로 그냥 아래처럼 쓰면 됨:

```js
const { log } = console;
```

## NPM

NPM(~~Node Package Manager~~ npm is not an acronym)은 Node.js의 모듈관리 도구다. https://npmjs.org

### 모듈 설치

```bash
# package.json 의 "dependencies"를 참조하여 자동설치
npm install

# 로컬 모듈로 설치
npm install 모듈1[, 모듈2, 모듈3, ...]

# 글로벌 모듈로 설치
npm install 모듈명 -g

# --save: package.json의 dependencies 항목에 해당 모듈을 추가한다.
# 사실 기본값이 true라서 생략해도 결과는 같음
npm install 모듈명 --save

# 해당 모듈은 'devDependencies'일 때만 사용된다. 즉, production 모드로 빌드 시 포함하지 않는다.
npm install 모듈명 --save-dev

# --save-dev와 비슷한데 이 경우는 'optionalDependencies'일 때만 사용
npm install 모듈명 --save-optional

# nodemon: js 파일의 내용이 변경되면 자동으로 재실행시키는 확장 모듈
npm install nodemon -g

# server.js를 nodemon으로 실행
nodemon server.js
```

### 조회

```bash
# 설치된 npm의 버전을 확인한다.
npm -v

# 현재 경로의 로컬 모듈 확인
npm ls

# 글로벌 모듈 확인
npm ls -g

# 로컬 모듈들의 종속관계 확인. global은 지원하지 않음
npm fund
```

`fund` 명령은 단순히 종속관계 목록을 출력하는게 아니라 웬 사이트 주소를 함께 표시해 주는데, 이 주소는 모듈 제작자에게 기부를 할 수 있는 페이지다. 그래서 이름이 `fund`인 것.

### 업데이트

```bash
# 모듈명을 명시 하지 않으면 로컬 모듈을 모두 업데이트
npm update [모듈명]

# 모듈명을 명시 하지 않으면 글로벌 모듈을 모두 업데이트
npm updata [모듈명] -g
```

### 모듈 삭제

```bash
# 로컬 모듈 삭제
npm uninstall 모듈명

# 글로벌 모듈 삭제
npm uninstall 모듈명 -g
```

### package.json 자동생성

```bash
npm init
```

### package.json 구성요소

npm init 으로 자동생성되는 요소들

- name: 프로젝트 이름. 배포 시 필수 항목
- version: 버전. 배포 시 필수 항목
- description: 프로젝트 설명
- main:
- scripts: 프로젝트에서 자주 실행될 명령어를 스크립트로 작성한다. (npm help scripts로 확인할 것)
- author: 작성자
- license:
- dependencies: 필요한 native 모듈 정보를 적는다. npm install 명령으로 자동 설치된다.
- devDependencies: 개발 시에만 필요한 모듈을 명시한다. config(package.json의 config와는 다르다. npm config list -l 명령으로 설정목록을 확인 할 수 있고 production 값을 바꾸려면 npm config set production true를 사용한다.)에 production이 true일 때는 배포버전이라 간주하고 설치하지 않는다.
- repository:
- keywords: 키워드

### 그 외 추가 가능한 요소

- homepage: 프로젝트의 홈페이지
- contributors: 공헌자 정보
- config: scripts가 사용할 수 있는 환경 변수
- private: npm 저장소 배포 여부. true로 지정하면 배포하지 않는다.
- engine: 프로젝트의 기반 엔진을 표시한다.

### example

```js
{
  "name": "jstest",
  "version": "0.0.0",
  "description": "example project",
  "main": "globaltest.js",
  "scripts": {
    "start" : "node app.js"
  },
  "author": {
    "name" : "noriter",
    "email" : "xxx@xxx.com"
  },
  "license": "none",
  "dependencies": {
    "express": "~3.3.5"
  },
  "devDependencies": {},
  "repository": {
    "type": "git",
    "url": "none"
  },
  "keywords": [
    "keywordA",
    "keywordB"
  ]
}
```

### 설치한 모듈 실행

[npm Docs: npm-exec](https://docs.npmjs.com/cli/v7/commands/npm-exec)

```bash
# 로컬에 설치한 mocha 모듈 실행
npm exec 모듈명
```

모듈을 전역으로 설치한게 아니라면 이 명령어로 실행해야함.

예시:

```bash
npm exec mocha test/**
npm exec http-server -p 9090
```

`npm exec`와 비슷한 [npx](https://docs.npmjs.com/cli/v7/commands/npx)가 있다. [npm Docs: npx vs npm](https://docs.npmjs.com/cli/v7/commands/npx#npx-vs-npm-exec)

### 모듈을 폴더단위로 관리하기

package.json을 조작하면 한 폴더에 있는 모듈을 마치 라이브러리 파일처럼 다룰 수 있다. 방법은 다음과 같다.

```js
var myModule = require('./myModule');
```

모듈 로드 시 위와 같은 방식으로 폴더를 지정할 수 있는데, 이 경우 노드는 해당 폴더 내에서 모듈을 찾는다. 노드는 이 폴더가 패키지라고 가정하고 패키지 정의를 찾는다. `package.json` 파일이 없다면 `index.js` 파일을 기준으로 루트라 가정한다. `/myModule/index.js` 가 있다면 노드는 myModule을 루트라 가정하고 그 경로 아래에서 파일을 찾는다.

package.json 에서 시작점의 상대경로를 지정하는 방법은:  
```js
{  
  "main": "./lib/temp.js"
}
```

이 경우 노드는 `./myModule/lib/temp.js` 를 찾는다.  
[관련 내용을 설명한 블로그](http://nodejs.sideeffect.kr/docs/v0.10.7/api/modules.html#modules_folders_as_modules)

### npm scripts

package.json에 스크립트를 등록해서 `npm x`같은 간략한 명령어로 미리 정해진 스크립트를 실행할 수 있다.

예를 들어 React.js는 튜토리얼용 패키지 설치 후 바로 `npm start`, `npm run build`, `npm test` 등의 명령어를 사용할 수 있는데, 이게 다 package.json에 `scripts` 항목으로 미리 등록되어 있기 때문에 가능한 것이다:

```js
{
  "name": "my-react-app",
  // 생략
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
  // 생략
}
```

요런 설정일 때 `npm start`는 `node node_modules/react-scripts/scripts/start.js`와 같다고 볼 수 있다.

## Yarn

NPM의 속도와 보안을 강화한 [Yarn](https://yarnpkg.com/)이 있음. [NPM vs. Yarn: Which Package Manager Should You Choose?](https://www.whitesourcesoftware.com/free-developer-tools/blog/npm-vs-yarn-which-should-you-choose/)

```bash
# NPM으로 Yarn 설치
npm install yarn -g
```

```bash
# Yarn으로 PACKAGE_NAME 설치
yarn add PACKAGE_NAME

# Yarn으로 PACKAGE_NAME 삭제
yarn remove PACKAGE_NAME
```

NPM을 완전히 대체하는 것은 아니라서 둘 다 각각 사용 가능.

### [Yarn Global](https://classic.yarnpkg.com/en/docs/cli/global)

```bash
# 패키지를 글로벌로 설치하되 설치 경로는 /usr/local로
yarn global add nodemon --prefix /usr/local

# 글로벌 패키지 확인
yarn global list
```

글로벌 설치 경로 기본값은 [NPM](https://nodejs.dev/learn/where-does-npm-install-the-packages)과 달라서 Yarn으로 설치한 글로벌 패키지가 NPM으로는 안보일 수 있다.

```bash
"$env:APPDATA\npm\node_modules"
# C:\Users\fixalot\AppData\Roaming\npm\node_modules
npm root -g
# C:\Program Files\nodejs\node_modules

yarn global dir
# C:\Users\fixalot\AppData\Local\Yarn\Data\global
```

만약 `yarn global add`로 설치했는데 해당 패키지의 명령어를 못찾는다고 하면 다음처럼 path에 글로벌 패키지의 bin 파일 모음 경로를 추가한다:

```bash
[Environment]::SetEnvironmentVariable("PATH", "$env:PATH;$(yarn global bin)", "User")
```

[NVM을 쓰는 경우 Yarn을 통한 글로벌 설치가 문제가 될 수 있다는 말이 있다](https://stackoverflow.com/questions/56941551/is-there-any-difference-between-installing-global-packages-with-yarn-or-npm).

실제 겪은 일: NVM을 쓰는 환경에서 Yarn 글로벌로 `react-devtools`를 설치했는데 React Native Debugger에서 자꾸 높은 버전으로 올리라고 함. NPM 글로벌로 설치했더니 해당 메시지 사라짐.

**그냥 글로벌 패키지는 NPM으로 하는게 좋을 것 같다**.
