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
- [package.json \| npm Docs](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [Yarn](https://yarnpkg.com/)


## 개요

npm과 Yarn 사용법 간단 정리


## package.json

`package.json`은 프로젝트(혹은 패키지)의 설명, 의존관계, 빌드/실행 스크립트 등을 정의하는 파일이다. `npm`이나 `yarn` 명령은 이 파일을 기반으로 의존성 설치나 스크립트 실행 등을 자동으로 처리한다.

직접 만들어도 되지만 `npm init` 명령으로 만드는 게 간편하다.

### 필드 설명

`init` 으로 자동생성되는 필드들은 다음과 같다:

- `name`: 프로젝트 이름. 배포 시 필수
- `version`: 버전. 배포 시 필수
- `description`: 프로젝트 설명
- `main`:
- `scripts`: 프로젝트에서 자주 실행될 명령어를 스크립트로 작성한다. (npm help scripts로 확인할 것)
- `author`: 작성자
- `license`:
- `dependencies`: 의존하는 패키지 정보를 적는다. npm install 명령으로 자동 설치된다.
- `devDependencies`: 개발 시에만 필요한 패키지를 명시한다. config(`package.json`의 `config` 필드와는 다르다. `npm config list -l` 명령으로 설정목록을 확인 할 수 있고 production 값을 바꾸려면 `npm config set production true`를 사용한다.)에 production이 `true`일 때는 배포버전이라 간주하고 설치하지 않는다.
- `repository`: 버전 관리 저장소 정보
- `keywords`: 키워드

#### 필요하면 추가할 수 있는 필드들

- `homepage`: 프로젝트의 홈페이지
- `contributors`: 공헌자 정보
- `config`: scripts가 사용할 수 있는 환경 변수
- `private`: npm 저장소 배포 여부. true로 지정하면 배포하지 않는다.
- `engine`: 프로젝트의 기반 엔진을 표시한다.

#### 패키지 매니저가 Yarn일 때만 작동하는 필드

[Manifest (package.json) \| Yarn](https://yarnpkg.com/configuration/manifest)

- `type`: `.js` 파일을 어떻게 해석(interpret)할지를 정의한다. `"module"`은 ESM, `"commonjs"`는 CJS를 의미한다.
- `resolutions`: 프로젝트 전체에서 특정 패키지의 버전을 강제로 지정한다. 모든 의존성 트리에 걸쳐 강제 적용되기 때문에, 여러 패키지들이 각각 요구하는 버전이 다를 경우 문제가 될 수 있다.

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

ℹ️ Node.js에서 패키지의 특정 버전을 가라키는 용어로 'resolution'이 사용된다. (특히 Yarn에서)

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

### 모듈을 디렉터리 단위로 관리하기

package.json을 조작하면 한 디렉터리에 있는 모듈을 마치 라이브러리 파일처럼 다룰 수 있다. 방법은 다음과 같다:

```js
var myModule = require('./myModule');
```

위와 같이 모듈을 로드할 때 디렉터리를 지정하면, Node.js는 그 디렉터리를 패키지로 가정하고, 내부에서 `package.json` 파일을 찾는다. 만약 `package.json`이 있다면 `main` 필드에 정의된 파일을 시작점으로 사용한다. `package.json` 파일이 없다면 `index.js` 파일을 패키지의 시작점으로 간주한다. 예를 들어 `/myModule/index.js`가 있다면, Node.js는 `/myModule`을 패키지의 루트 경로로 보고 해당 파일을 로드한다.

만약 `package.json` 에서 다음처럼 시작점의 상대경로를 지정했다면:

```js
{
  "main": "./lib/temp.js"
}
```

Node.js는 `./myModule/lib/temp.js` 를 찾는다.

[관련 내용을 설명한 블로그](http://nodejs.sideeffect.kr/docs/v0.10.7/api/modules.html#modules_folders_as_modules)


## npm

npm(~~Node Package Manager~~ npm is not an acronym)은 Node.js의 공식 패키지(혹은 모듈) 관리 도구다.

### ⚠️ 해킹 방지 부적(?)

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

### 패키지(Package) 설치

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

# --save: package.json의 dependencies 필드에 해당 패키지를 추가한다.
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

### 설치된 패키지 조회

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

### 패키지 버전 업그레이드

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

### 패키지 삭제

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

### npm exec

[npm Docs \| npm-exec](https://docs.npmjs.com/cli/v8/commands/npm-exec)

설치한 패키지 실행

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

#### npm exec와 npx의 차이

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


## Yarn

npm의 속도와 보안을 강화한 새 패키지 매니저. 속도가 빠른 이유는 병렬로 수행해서 그런다나 뭐라나.

```bash
corepack enable

# 코어팩이 없을 경우
npm install -g corepack
```

[코어팩(Corepack)](https://yarnpkg.com/corepack) 프로젝트에 Yarn이 포함되어있으니 따로 설치할 필요 없이 코어팩만 활성화하면 된다. 코어팩은 패키지 매니저(Yarn, npm, pnpm)의 버전 관리를 위한 도구로, Node.js 16.13.0 버전 이상이면 자동으로 설치된다.

```bash
# Yarn으로 Node 실행(현재 프로젝트 환경에 호환되는 방식으로)
yarn node ./SCRIPT_NAME

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

# 패키지를 다시 받아오고 체크섬을 확인한다(=캐시 무시 재설치)
yarn install --check-cache

# 패키지 버전(resolution)은 그대로 유지하면서 패키지 메타데이터 갱신
yarn install --refresh-lockfile

# 직접 설치된 모든 패키지의 버전, 실행 가능한 파일, 의존성을 표시
yarn info

# PACKAGE_NAME에 대한 버전, 실행 가능한 파일, 의존성을 표시
yarn info PACKAGE_NAME

# Yarn으로 PACKAGE_NAME 삭제
yarn remove PACKAGE_NAME

# 프로젝트의 모든 패키지 버전 업그레이드
yarn up

# PACKAGE_NAME 패키지의 버전 업그레이드
yarn up PACKAGE_NAME

# 대화형으로 버전 업그레이드
yarn upgrade-interactive

# 캐시 지우기
yarn cache clean
```

🚨 Yarn으로 패키지를 추가/삭제/업그레이드 해도 `package.json` 내용이 수정된다.

ℹ️ `yarn` 명령은 하위 명령어가 없으면 `yarn install`, 있으면 `yarn run`으로 작동한다.

### yarn.lock

Yarn은 `yarn.lock`이라는 별도의 lock 파일(패키지 잠금 파일이라고도 함)을 `add` 혹은 `install` 시 자동으로 생성한다.

기본적인 역할은 npm의 `package-lock.json` 파일과 같은데, 실제 설치한 패키지의 버전을 기록하며 이 lock 파일이 있다면 `install` 시 정확히 동일한 버전의 패키지를 설치한다. 따라서 개발자들끼리의 소스 공유 시엔 이 파일을 반드시 버전 관리에 추가해야 한다.

`--no-lockfile` 등의 옵션으로 lock 파일을 생성하지 않게 할 수도 있다. 설치만 Yarn으로 할 때 쓰려나...?

### .yarnrc.yml

`.yarnrc.yml` 파일은 Yarn 설정 파일로 2.x 버전 이상부터 사용된다. 

주요 필드로:

- `nodeLinker`: Node 패키지 설치 방법을 정의한다. `pnp`, `pnpm`, `node-modules`(기존 방식) 중 택일.
- `packageExtensions`: 특정 패키지의 추가적인 의존성을 지정하거나 기존의 필드를 수정하기 위해 사용한다. `package.json`의 `resolutions` 필드와 달리 특정 패키지에만 적용 가능. 

```yml
packageExtensions:
  "@babel/core@*":
    dependencies:
      "@babel/types": "*"
```

이 외의 설정 가능한 필드는 `yarn config -v` 명령으로 조회 하던지 아니면 [이 문서](https://yarnpkg.com/configuration/yarnrc)를 보자.

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

### yarn init vs yarn create

npm과 다르게 Yarn은 `init`과 `create`의 행동이 다르다(npm의 `create`는 `init`의 별칭이다).

`yarn init`은 현재 경로에 단순한 Node.js 프로젝트를 생성할 때 사용한다. `package.json`, `.gitignore` 등의 기본적인 파일들이 생성된다.

`yarn create`는 특정 템플릿이나 스타터 킷을 기반으로 프로젝트를 스케폴딩 할 때 사용한다.

```bash
# Vite 기반 프로젝트 만들기
yarn create vite
```

### yarn run

```
yarn run [script] [<args>]
```

npm scripts로 정의한 명령어를 실행한다.

```bash
yarn run live-server --open=out
```

만약 `script`가 npm scripts에 정의되어 있지 않은 키워드라면, 로컬에 설치된 패키지의 실행 파일(`node_modules/.bin/`의 바이너리 파일)을 찾는다. 이것도 없으면 워크스페이스의 스크립트를 찾는다.

### yarn dlx

`dlx`는 `npx`처럼 어떤 패키지를 임시 환경에 설치하고 (바이너리 스크립트가 포함된 패키지인 경우) 실행하는 명령이다.

⚠️ Yarn을 `npm install yarn -g` 명령으로 설치했다면 `yarn dlx`를 실행했을 때 `package.json`이 없다는 에러가 발생한다. 글로벌로 설치된 Yarn은 지우고 `corepack enable`로 코어팩을 활성화 할 것.

```
yarn dlx <command> ...
```

```bash
# my-app 디렉터리에 CRA로 리액트 앱 스캐폴딩
yarn dlx create-react-app ./my-app
```

도움말을 보면 `add` 대신 사용하지 말라고 권장한다:

> Using yarn dlx as a replacement of yarn add isn't recommended, as it makes your project non-deterministic (Yarn doesn't keep track of the packages installed through dlx - neither their name, nor their version).

### yarn explain peer-requirements

```
yarn explain peer-requirements [hash]
```

패키지들 간의 peer dependency(설치한 패키지가 의존하는 또 다른 패키지) 요구사항과 현재 상태를 분석해서 보여주는 명령어. 의존성 문제를 디버깅할 때 사용한다.

`hash`를 생략하고 사용하면 모든 항목과 해시값을 출력한다. 여기서 출력되는 해시를 `hash`로 지정하면 더 상세한 내용이 출력된다.

문제가 있는 경우 `yarn why` 등으로 원인을 확인한 뒤, 상황에 따라 필요한 패키지를 추가하거나 `package.json`의 `resolutions` 혹은 `.yarnrc.yml`의 `packageExtensions` 필드로 의존성 설정을 적절히 수정하여 해결하면 된다.

ℹ️ 여기서 말하는 의존성 문제란 대부분 ghost dependencies를 의미한다. Ghost dependencies란 하위 패키지에서 의존하는 또 다른 패키지를 명시적인 선언 없이 프로젝트에서 참조하는 것을 말한다. Ghost dependencies는 불안정한 빌드를 야기하거나 업데이트 문제 등이 발생시킬 수 있기 때문에 고치는게 좋다. Yarn PnP는 ghost dependencies를 방지하기 위해 명시적으로 의존하지 않는 패키지는 자동으로 참조하지 않는다.

### yarn why

```
yarn why <query>
```

특정 패키지가 왜 설치되었는지 또는 어떤 패키지에 의해서 설치된 것인지를 보여준다.

`query`는 패키지 이름이나 패키지의 디렉터리명, 패키지의 디렉터리명 + 파일이름으로 지정할 수 있다.

### Yarn PnP(Plug'n'Play)

[Plug'n'Play \| Yarn](https://yarnpkg.com/features/pnp)

Yarn PnP는 Yarn 2.x 버전 이상부터 사용 가능한 Yarn의 새로운 패키지 설치 방식이다. 전통적인 `node_modules` 방식과 다르게, 의존하는 패키지를 하나의 압축 파일 형태로 저장한다.

PnP는 기존보다 적은 용량으로 더 빠르게 설치되며, 의존성 충돌 문제를 방지하고, 의존성 문제가 발생했을 때 디버깅이 쉽다는 장점이 있다. 다만 일부 패키지는 아직 PnP 환경에 호환되지 않을 수 있으니 이 점은 주의할 것. (특히 리액트 네이티브와 Expo가 그렇다)

PnP는 Yarn 버전 2.x 이상이며 `.yarnrc.yml` 파일이 있고 `nodeLinker` 필드가 `pnp`일 때, 혹은 `.yarnrc.yml` 파일이 아예 없을 때 자동으로 활성화된다. 활성화 상태라면 `yarn install` 시 `.yarn` 디렉터리, `.pnp.cjs`, `.pnp.loader.mjs` 파일 등이 자동으로 생성된다.

`yarn --version`으로 버전을 확인했을 때 2.x 아래면 `yarn set version berry` 명령으로 상위 버전을 지정하면 된다. 이 명령은 `package.json`의 `packageManager` 필드값을 Yarn의 최신 버전으로 변경한다. 참고로 berry는 Yarn 2.x 버전부터 시작된 대규모 업데이트의 코드네임으로, Yarn 2.x 이상 버전을 지칭한다.


## Node.js 패키지 모음

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

### Node-Tap

[https://www.npmjs.com/package/tap](https://www.npmjs.com/package/tap)

웹 애플리케이션 백엔드 코드용 테스트 프레임워크

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
