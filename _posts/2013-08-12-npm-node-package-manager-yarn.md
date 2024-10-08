---
layout: post
date: 2013-08-12 17:00:00 +0900
title: '[Node.js] npm, Yarn'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - npm
  - yarn
  - package-manager
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [npm](https://www.npmjs.com/)
- [npm Docs](https://docs.npmjs.com/)


## 개요

npm(~~Node Package Manager~~ npm is not an acronym)은 Node.js의 모듈 관리 도구다.


## ⚠️ 해킹 방지 부적(?)

npm의 패키지 관리와 종속성을 처리하는 방식은 해킹에 매우 취약하여 공격자의 스크립트가 개발자의 PC에서 실행될 수 있다. 수없이 얽히고 설킨 종속 패키지 중 하나를 이용해 공격하는 방식이 주로 사용되는데 Supply Chanin Attack 이라 한다고... 

이를 방지하기 위해 아래 방법들이 권장된다:

- 꼭 필요한 써드파티 라이브러리만 사용할 것
- 패키지 설치 시 오타에 주의하고 패키지 게시자를 확인할 것
- `npm install` 실행 시 항상 `--ignore-scripts` 옵션을 붙일 것

```bash
npm insatll --ignore-scripts
```

`--ignore-scripts`는 npm이 package.json에 작성된 pre-scripts, post-scripts를 자동으로 실행하지 않도록 하는 옵션이다.

\* Node.js 20부터는 권한을 관리할 수 있는 기능이 추가되었다고 한다.


## 패키지(Package) 설치

```bash
npm install [<package-spec> ...]
# 별칭: add, i, in, ins, inst, insta, instal, isnt, isnta, isntal, isntall 
```

오타까지 별칭으로 해놓은 건... 좀 웃겼다 🤭

```bash
# package.json에 작성된 의존 패키지를 모두 설치
# package-lock.json이 있으면 해당 의존성을 우선함
npm install

# 의존 패키지를 모두 설치하되 devDependencies의 패키지는 제외
npm install --production

# 로컬 패키지로 설치
npm install 패키지1[, 패키지2, 패키지3, ...]

# 글로벌 패키지로 설치
npm install 패키지 -g

# --save: package.json의 dependencies 항목에 해당 패키지를 추가한다.
# 사실 기본값이 true라서 생략해도 결과는 같음
npm install 패키지 --save

# 해당 패키지는 'devDependencies'일 때만 사용된다. 즉, production 모드로 빌드 시 포함하지 않는다.
npm install 패키지 --save-dev

# --save-dev와 비슷한데 이 경우는 'optionalDependencies'일 때만 사용
npm install 패키지 --save-optional

# node_modules, package.json 생성 경로 지정
npm install 패키지 --prefix .

# nodemon: js 파일의 내용이 변경되면 자동으로 재실행시키는 패키지
npm install nodemon -g

# PACKAGE_NAME 패키지를 최신버전으로 설치
npm install PACKAGE_NAME@latest

# 지정한 버전 번호와 일치하는 버전으로 설치
npm install PACKAGE_NAME@1.2.3

# 지정한 태그와 일치하는 버전으로 설치
npm install PACKAGE_NAME@tag
```


## 조회

```bash
npm ls <package-spec>
# 별칭: list
```

```bash
# 설치된 npm의 버전을 확인한다.
npm -v

# 현재 경로의 로컬 패키지 확인
npm ls

# 설치된 글로벌 패키지 확인
npm ls -g

# 설치된 로컬 패키지들의 종속관계 확인. global은 지원하지 않음
npm fund
```

`fund` 명령은 단순히 종속관계 목록을 출력하는게 아니라 웬 사이트 주소를 함께 표시해 주는데, 이 주소는 패키지 제작자에게 기부를 할 수 있는 페이지다. 그래서 이름이 `fund`인 것.


## 패키지 버전 업그레이드

```
npm update [<pkg>...]
```

별칭: `up`, `upgrade`, `udpate`

```bash
# 버전 업그레이드 가능한 패키지의 버전 정보 표시: : 현재 설치된 버전, 원하는 버전, 사용 가능한 최신 버전, 로컬 설치 경로, 종속성
npm outdated

# 특정 로컬 패키지의 버전을 업그레이드
npm update [패키지명]

# 특정 글로벌 패키지의 버전을 업그레이드
npm update [패키지명] -g

# 모든 패키지의 버전을 업그레이드 하고 package.json의 의존성 업데이트
npm update --save
```

패키지가 아니라 Node.js 클라이언트의 버전을 올리고 싶으면 [NVM](https://github.com/nvm-sh/nvm)을 설치할 것.


## 패키지 삭제

```bash
npm uninstall [<@scope>/]<pkg>...
# 별칭: unlink, remove, rm, r, un
```

```bash
# 로컬 패키지 삭제
npm uninstall 패키지명

# 글로벌 패키지 삭제
npm uninstall 패키지명 -g
```


## 설치한 패키지 실행

[npm Docs \| npm-exec](https://docs.npmjs.com/cli/v8/commands/npm-exec)

```bash
# 로컬에 설치한 mocha 패키지 실행
npm exec 패키지명
```

패키지를 전역으로 설치한게 아니라면 이 명령어로 실행해야함.

예시:

```bash
npm exec mocha test/**
npm exec http-server -p 9090
```

아니면 [npx](https://docs.npmjs.com/cli/v8/commands/npx)로 실행해도 된다: 

```bash
npx mocha test/**
npx http-server -p 9090
```

### npm exec와 npx의 차이

[npm Docs \| npx vs npm exec](https://docs.npmjs.com/cli/v8/commands/npm-exec#npx-vs-npm-exec)

우선 `npx`는 명령 실행에 필요한 패키지가 로컬에 없으면 자동으로 어딘가(OS마다 다른 temp 디렉터리)에 다운로드한다.

그리고 `npx`는 실행할 명령어 뒤에 오는 옵션을 명령어의 인수로 전달하지만, `npm exec`는 `npm`의 옵션으로 먼저 처리한다. 

다음처럼 작성했을 때:

```bash
npm exec foo --package=@npmcli/foo
```

`--package` 옵션은 `foo` 명령의 인수가 아니라 `npm`의 옵션으로 처리된다. 이를 억제하려면 이중 하이픈`--`을 덧붙이면 된다.

예를 들면 아래 두 명령은 같다:

```bash
npx tap --bail test/foo.js
npm exec -- tap --bail test/foo.js
```

아래 두 명령도 같다:

```bash
npx nodemon --exec tsc
npm exec -- nodemon -- --exec tsc
```

🤔 가이드 문서 대로면 `npm exec -- nodemon --exec tsc`라고 해야 맞는데 어째서인지 `npm exec -- nodemon -- --exec tsc`라고 이중 하이픈을 한 번 더 해줘야 한다.

#### 하이픈 두 번의 뜻 `--`

`--`는 옵션의 끝을 알리고 추가 옵션 처리를 비활성화하는 구분자다. `--` 뒤의 `공백+문자`가 앞에(왼쪽에) 명시한 명령어의 옵션이 아니라 다른 명령어 혹은 파일이나 디렉터리라는 것을 인터프리터에게 알리기 위해 사용한다.

Yarn은 이런 거 안해도 `yarn run` 명령으로 그냥 된다. npx 싫으면 Yarn 쓰자.


## package.json

`package.json`은 프로젝트(혹은 패키지)의 설명, 의존관계, 빌드/실행 스크립트 등을 정의하는 파일이다. 직접 만들어도 되지만 보통은 `init`을 씀:

### npm init

```bash
npm init
npm init <package-spec> (same as `npx <package-spec>)
npm init <@scope> (same as `npx <@scope>/create`)

별칭: create, innit
```

`npm init`은 프로젝트를 새로 만들거나, 기존 패키지를 재설정할 때 사용하는 명령어다.

이 명령을 추가 인자 없이 실행하면 패키지 이름, 버전, 설명 등을 물어보는 프롬프트가 나타난다. 끝까지 입력을 마치면 입력한 내용대로 `package.json` 파일이 만들어진다. 이미 `package.json` 파일이 있으면 내용을 수정한다.

`init` 뒤에 패키지 이름을 붙여 특정 템플릿이나 스타터 킷으로 프로젝트를 스케폴딩 하기도 한다:

```bash
# Vite 기반 프로젝트 만들기
npm init vite@latest
```

이런식으로 패키지를 지정하면 `npm exec` 명령으로 바뀐다고 한다. [자세한 내용은 이 링크를 보자](https://docs.npmjs.com/cli/v9/commands/npm-init#description).

### package.json 구성요소

`init` 으로 자동생성되는 항목은 다음과 같다:

- name: 프로젝트 이름. 배포 시 필수 항목
- version: 버전. 배포 시 필수 항목
- description: 프로젝트 설명
- main:
- scripts: 프로젝트에서 자주 실행될 명령어를 스크립트로 작성한다. (npm help scripts로 확인할 것)
- author: 작성자
- license:
- dependencies: 의존하는 패키지 정보를 적는다. npm install 명령으로 자동 설치된다.
- devDependencies: 개발 시에만 필요한 패키지를 명시한다. config(package.json의 config와는 다르다. npm config list -l 명령으로 설정목록을 확인 할 수 있고 production 값을 바꾸려면 npm config set production true를 사용한다.)에 production이 true일 때는 배포버전이라 간주하고 설치하지 않는다.
- repository:
- keywords: 키워드

### 필요하면 추가할 수 있는 항목들

- homepage: 프로젝트의 홈페이지
- contributors: 공헌자 정보
- config: scripts가 사용할 수 있는 환경 변수
- private: npm 저장소 배포 여부. true로 지정하면 배포하지 않는다.
- engine: 프로젝트의 기반 엔진을 표시한다.
- TODO

### 패키지 버전 지정하기 version numbering

```
"dependencies": {
  "express": "^3.3.5"
}
```

[이 문서](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#dependencies)에 설명된 것 중 중요한 패턴만 추려보면:

- `*`: 아무 버전이나 찾는다.
- `""`: 빈 문자열이며 `*`와 동일함.
- `1.2.x`: x로 지정한 단위는 아무 버전이나 허용한다. `1.x`는 메이저 버전이 1이면 다 괜찮다는 뜻이다.
- `version`: 지정된 버전과 정확히 일치하는 버전을 허용한다.
- `>version`: 지정된 버전보다 높은 버전을 허용한다.
- `>=version`: 지정된 버전보다 높거나 같은 버전을 허용한다.
- `<version`: 지정된 버전보다 낮은 버전을 허용한다.
- `<=version`: 지정된 버전보다 낮거나 같은 버전을 허용한다.
- `~version`: '대략적으로 동일한 버전'을 허용한다. `version`을 명시한 단위보다 낮은 단위는 아무 버전이나 괜찮다는 뜻이다. 예를 들어 `~0.2.3`은 0.2.3보다 높거나 같고 0.3.0보다는 낮은 버전을 허용한다. `~1.2`는  `1.2.x`와 같다. `~0`은 `0.x`와 같다.
- `^version`: '호환되는 버전'만 허용한다. 지정한 버전의 **0이 아닌 가장 왼쪽에 있는 숫자**가 변하지 않는 선에서 같거나 높은 버전을 허용한다. `^1.2.3`은 1.2.3보다 높거나 같고 2.0.0보다 낮아야 한다. `^0.2.3`은 0.2.3보다 높거나 같고 0.3.0보다 낮아야 한다. `^0.0.3`은 `>=0.0.3 <0.0.4-0`라고 하는 것과 같다. 더 자세한 내용은 [여기](https://github.com/npm/node-semver#caret-ranges-123-025-004)를 보자.

참고로 npm은 [Semantic Versioning](https://semver.org/)(줄여서 SemVer)을 따른다.

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

### npm scripts

`package.json`의 `scripts`를 이용해서 `npm start`와 같은 간략한 명령어로 미리 작성한 스크립트를 실행하게 하는 기능이다.

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

요런 설정일 때 `npm start`는 `node node_modules/react-scripts/scripts/start.js` 명령을 실행하는 것과 같다.

단, **프로퍼티 이름이 `start`, `test`, `build`, `dev`, `lint`, `precommit`일 경우에만 `npm 스크립트_이름` 형태로 실행할 수 있다**. 이 외의 이름은 사용자 정의 스크립트로 취급되며 `run` 키워드를 붙여 `npm run 스크립트_이름`으로 실행해야 한다.

```js
{
  "name": "my-app",
  // 생략
  "scripts": {
    "hello": "echo 'Hello world!'"
  }
  // 생략
}
```

```bash
> npm run hello

> lab-js@1.0.0 hello
> echo 'Hello world!'

'Hello world!'
```

`npx`를 사용하고 싶지 않을때는 `npm exec`를 사용해야 하는데, 이러면 명령어의 옵션 지정이 굉장히 번거롭기 때문에 스크립트로 등록하는 편이 좋다.

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


## Yarn

[Yarn](https://yarnpkg.com/)

npm의 속도와 보안을 강화한 새 패키지 매니저. [npm vs. Yarn: Which Package Manager Should You Choose?](https://www.whitesourcesoftware.com/free-developer-tools/blog/npm-vs-yarn-which-should-you-choose/)

속도가 빠른 이유는 병렬로 수행해서 그런다나 뭐라나

```bash
corepack enable

# 코어팩이 없을 경우
npm install -g corepack
```

ℹ️ 예전에는 `npm install yarn -g`라고 적어놨지만, 다시 찾아보니 코어팩(Corepack) 프로젝트에 Yarn이 포함되었으니 코어팩 활성화만 하면 된다고 한다. 만약 코어팩이 없다면 npm으로 설치하자. `npm install -g corepack` [관련글 링크](https://yarnpkg.com/corepack)

```bash
# Yarn으로 PACKAGE_NAME 설치 후 package.json에 추가
yarn add PACKAGE_NAME

# 지정한 버전 번호와 일치하는 버전으로 설치
yarn add PACKAGE_NAME@1.2.3

# 지정한 태그(e.g. beta, next, latest)와 일치하는 버전으로 설치
yarn add PACKAGE_NAME@TAG_NAME

# 개발환경(devDependencies)에서만 유효한 패키지 설치
yarn add --dev PACKAGE_NAME
# yarn add -D PACKAGE_NAME

# package.json에 작성된 의존 패키지를 모두 설치
# yarn.lock이 있으면 해당 의존성을 우선함
yarn install

# 의존 패키지를 모두 설치하되 devDependencies의 패키지는 제외
yarn install --production

# 로컬 경로(yarn.lock이 위치한 디렉터리)에 설치된 패키지 목록 출력
# yarn.lock 파일이 없으면 작동하지 않음
yarn list

# 의존관계 깊이가 1인 패키지만 출력(=직접 설치한 패키지만 출력)
yarn list --depth 1

# Yarn으로 PACKAGE_NAME 삭제
yarn remove PACKAGE_NAME

# 업그레이드 가능한 패키지의 버전 정보를 표시: 현재 설치된 버전, 원하는 버전, 사용 가능한 최신 버전, 종속성 타입, URL
yarn outdated

# 특정 패키지의 버전 업그레이드
yarn upgrade PACKAGE_NAME

# 대화형으로 버전 업그레이드
yarn upgrade-interactive

# 모든 패키지를 package.json을 무시하고 가장 최신 버전으로 업그레이드. package.json의 의존성도 업데이트 한다.
yarn upgrade --latest

# 캐시 지우기
yarn cache clean
```

Yarn으로 패키지를 추가하거나 삭제해도 `package.json` 내용이 수정되니 주의할 것.

`yarn install`의 단축어는 `yarn`이다.

### yarn init vs yarn create

npm과 다르게 yarn은 `init`과 `create`의 행동이 다르다(npm의 `create`는 `init`의 별칭이다).

`yarn init`은 현재 경로에 단순한 Node.js 프로젝트를 생성할 때 사용한다. `package.json`, `.gitignore` 등의 기본적인 파일들이 생성된다.

`yarn create`는 특정 템플릿이나 스타터 킷을 기반으로 프로젝트를 스케폴딩 할 때 사용한다.

```bash
# Vite 기반 프로젝트 만들기
yarn create vite
```

### yarn.lock

Yarn은 `yarn.lock`이라는 별도의 lock 파일(패키지 잠금 파일이라고도 함)을 `add` 혹은 `install` 시 자동으로 생성한다.

기본적인 역할은 npm의 `package-lock.json` 파일과 같은데, 실제 설치한 패키지의 버전을 기록하며 이 lock 파일이 있다면 `install` 시 정확히 동일한 버전의 패키지를 설치한다. 따라서 개발자들끼리의 소스 공유 시엔 이 파일을 반드시 버전 관리에 추가해야 한다.

`--no-lockfile` 등의 옵션으로 lock 파일을 생성하지 않게 할 수도 있다. 설치만 Yarn으로 할 때 쓰려나...?

### Yarn Global

참고: https://classic.yarnpkg.com/en/docs/cli/global

```bash
# 패키지를 글로벌로 설치하되 설치 경로는 /usr/local로
yarn global add PACKAGE_NAME --prefix /usr/local

# 글로벌 패키지 확인
yarn global list

# 글로벌로 설치한 패키지 삭제
yarn global remove PACKAGE_NAME
```

글로벌 설치 경로 기본값은 [npm](https://nodejs.dev/learn/where-does-npm-install-the-packages)과 달라서 Yarn으로 설치한 글로벌 패키지가 npm으로는 안보일 수 있다.

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

실제 겪은 일: NVM을 쓰는 환경에서 Yarn 글로벌로 `react-devtools`를 설치했는데 React Native Debugger에서 자꾸 높은 버전으로 올리라고 함. npm 글로벌로 설치했더니 해당 메시지 사라짐. (2022-01-28, Yarn v1.22.17)

**그냥 글로벌 패키지는 npm으로 하는게 좋을 것 같음.**

### yarn dlx = Yarn의 npx

⚠️ Yarn을 `npm install yarn -g` 명령으로 설치했다면 `yarn dlx`를 실행했을 때 `package.json`이 없다는 에러가 발생한다. 글로벌로 설치된 Yarn은 지우고 `corepack enable`로 코어팩을 활성화 할 것.

`dlx`는 `npx`처럼 어떤 패키지를 임시 환경에 설치하고 (바이너리 스크립트가 포함된 패키지인 경우) 실행하는 명령이다.

```
yarn dlx <command> ...
```

```bash
# my-app 디렉터리에 CRA로 리액트 앱 스캐폴딩
yarn dlx create-react-app ./my-app
```

도움말을 보면 `add` 대신 사용하지 말라고 권장한다:

> Using yarn dlx as a replacement of yarn add isn't recommended, as it makes your project non-deterministic (Yarn doesn't keep track of the packages installed through dlx - neither their name, nor their version).

### 기타

- [Yarn berry](https://www.npmjs.com/package/yarn-berry)를 쓰면 실행환경에 따라 발생하는 문제에서 npm보다 낫고 제로인스톨이라는게 좋다는 말이 있다.
- tap 패키지를 `npm`으로 설치하면 `^1.0.0`으로, `yarn`으로 설치하면 `^16.3.8`이 설치되는 이상한 현상이 있다. (2023-08-21)


## 자주 쓰는 패키지 모음

### nodemon

- [https://nodemon.io/](https://nodemon.io/)
- [https://github.com/remy/nodemon](https://github.com/remy/nodemon)

```bash
nodemon server.js
```

nodemon은 파일의 변경을 감지하면 자동으로 특정 명령을 재실행해주는 기능을 제공한다. 위 명령은 명령을 실행한 경로를 기준으로 현재 경로와 하위 경로의 파일들이 변경되었을 때 자동으로 `server.js` 파일을 다시 실행하라는 의미다.

`--watch` 옵션은 파일 변경을 감시할 경로를 별도로 지정하는 옵션이다. 예를 들어 nodemon은 기본값으로 상위 경로는 감시하지 않는데, `--watch` 옵션으로 지정하는게 가능하다:

```bash
# 상위 경로를 감시 대상으로 포함하며 server.js 실행
nodemon --watch ../ server.js
```

기본적으로 `.js`, `.mjs`, `.coffee`, `.litcoffee`, `.json` 확장자인 파일만 감시한다. (여기엔 실행 파일의 확장자도 포함된다. `watch.me` 파일을 실행중이면 `.me` 확장자도 감시하는 식) 

만약 감시할 확장자를 추가하려면 `--ext` 옵션을 사용하면 된다:

```bash
# .me 확장자와 .pug 확장자도 감시
nodemon --ext me,pug
```

`--exec` 옵션은 변경을 감지했을 때 실행할 명령어를 지정할 때 사용한다. 지정된 명령어는 스크립트 실행을 대체한다:

```bash
# 파일이 변경되면 tsc 실행
nodemon --exec 'tsc'
```

### mocha

**TODO**

### chai

**TODO**

### express

**TODO**

### Node-Tap

[https://www.npmjs.com/package/tap](https://www.npmjs.com/package/tap)

웹 애플리케이션 백엔드 코드용 테스트 프레임웍

### figlet

[https://www.npmjs.com/package/figlet](https://www.npmjs.com/package/figlet)

요 아래처럼 생긴 FIGFont를 만들어주는 패키지

```
  _____ _____  __     _      _     ___ _____ 
 |  ___|_ _\ \/ /    / \    | |   / _ \_   _|
 | |_   | | \  /    / _ \   | |  | | | || |  
 |  _|  | | /  \   / ___ \  | |__| |_| || |  
 |_|   |___/_/\_\ /_/   \_\ |_____\___/ |_|  
                                                 
```
