---
layout: post
date: 2013-08-12 17:00:00 +0900
title: '[Node.js] NPM, Yarn'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - npm
  - yarn
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [npm](https://www.npmjs.com/)
- [npm Docs](https://docs.npmjs.com/)


## 개요

NPM(~~Node Package Manager~~ npm is not an acronym)은 Node.js의 모듈관리 도구다.


## 모듈 설치

```bash
npm install [<package-spec> ...]
# 별칭: add, i, in, ins, inst, insta, instal, isnt, isnta, isntal, isntall 
```

오타까지 별칭으로 해놓은 건... 좀 웃겼다 🤭

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

# node_modules, package.json 생성 경로 지정
npm install 모듈명 --prefix .

# nodemon: js 파일의 내용이 변경되면 자동으로 재실행시키는 확장 모듈
npm install nodemon -g

# vue 패키지 최신버전으로 설치
npm install vue@latest
```


## 조회

```bash
npm ls <package-spec>
# 별칭: list
```

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


## 업데이트

```bash
npm update [<pkg>...]
# 별칭: up, upgrade, udpate
```

```bash
# 모듈명을 명시 하지 않으면 로컬 모듈을 모두 업데이트
npm update [모듈명]

# 모듈명을 명시 하지 않으면 글로벌 모듈을 모두 업데이트
npm update [모듈명] -g
```


## 모듈 삭제

```bash
npm uninstall [<@scope>/]<pkg>...
# 별칭: unlink, remove, rm, r, un
```

```bash
# 로컬 모듈 삭제
npm uninstall 모듈명

# 글로벌 모듈 삭제
npm uninstall 모듈명 -g
```


## 설치한 모듈 실행

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


## package.json

`package.json`은 프로젝트(혹은 모듈)의 설명, 의존관계, 빌드/실행 스크립트 등을 정의하는 파일이다. 직접 만들어도 되지만 보통은 `init`을 씀:

```bash
npm init
npm init <package-spec> (same as `npx <package-spec>)
npm init <@scope> (same as `npx <@scope>/create`)

별칭: create, innit
```

### package.json 구성요소

`init` 으로 자동생성되는 요소들

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


## 모듈을 폴더단위로 관리하기

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


## npm scripts

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

### Yarn Global

참고: https://classic.yarnpkg.com/en/docs/cli/global

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

실제 겪은 일: NVM을 쓰는 환경에서 Yarn 글로벌로 `react-devtools`를 설치했는데 React Native Debugger에서 자꾸 높은 버전으로 올리라고 함. NPM 글로벌로 설치했더니 해당 메시지 사라짐. (2022-01-28, Yarn v1.22.17)

**그냥 글로벌 패키지는 NPM으로 하는게 좋을 것 같음.**

뱀발: [Yarn berry](https://www.npmjs.com/package/yarn-berry)를 쓰면 실행환경에 따라 발생하는 문제에서 NPM보다 낫고 제로인스톨이라는게 좋다는 말이 있다.


## 자주 쓰는 패키지

### nodemon

```bash
# server.js를 nodemon으로 실행
nodemon server.js
```

TODO

### mocha

TODO

### chai

TODO

### express

TODO
