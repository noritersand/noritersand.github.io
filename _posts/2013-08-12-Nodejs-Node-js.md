---
layout: post
date: 2013-08-12 16:36:00 +0900
title: "Node.js"
categories:
  - javascript
  - node.js
tags:
  - node.js
---

![](/images/node-js-1.png)
> node에서는 모든게 병렬로 수행된다. 당신 code만 빼고

#### 참고한 글

- https://nodejs.org/en/docs/
- http://www.nodebeginner.org/index-kr.html


## 시작하기

### 설치

아래 링크에서 OS에 맞는 파일을 받아 설치한다.
https://nodejs.org/en/download/

안될 경우 `NODE_PATH = C:\Users\noriter\AppData\Roaming\npm\node_modules` 패스 추가.

### 구동

```bash
node 파일명
```

node 명령어는 단독으로 사용했을 때 REFL이라고 하는 node.js shell로 진입한다. 위 처럼 파일명과 같이 사용하면 해당 파일을 node.js로 실행하라는 의미이다.

### sync/async

동기 방식의 blocking I/O가 아래처럼 동작한다면:

```js
puts("이름을 입력하세요");
var name = gets();
puts("당신의 이름은" + name + "입니다.");
```
비동기 방식의 non-blocking I/O는 아래와 같이 동작한다:
```js
puts("이름을 입력하세요");
gets(function(name) {
  puts("당신의 이름은" + name + "입니다.");
});
```

### 이벤트 루프란?

프로세스의 처리과정을 이벤트의 순환으로 구현한 것 혹은, 이벤트를 처리하는 스택구조 자체를 이벤트루프라 함.

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

## npm

~~Node Package Manager~~ npm is not an acronym. Node.js의  모듈관리 도구다. https://npmjs.org

### 모듈 설치

```bash
node install
# package.json 의 "dependencies"를 참조하여 자동설치

npm install 모듈1[, 모듈2, 모듈3]
# 로컬 모듈로 설치

npm install 모듈명 -g
# 글로벌 모듈로 설치

npm install 모듈명 --save
# package.json 이 있다면 dependencies 항목에 설치한 모듈을 추가한다.

npm install nodemon -g
# nodemon: js 파일의 내용이 변경되면 자동으로 재실행시키는 확장 모듈

nodemon server.js
# server.js를 nodemon으로 실행
```

### 조회

```bash
npm -v
# 설치된 npm의 버전을 확인한다.

npm ls
# 현재 경로의 로컬 모듈 확인

npm ls -g
# 글로벌 모듈 확인
```

### 업데이트

```bash
npm update [모듈명]
# 모듈명을 명시 하지 않으면 로컬 모듈을 모두 업데이트

npm updata [모듈명] -g
# 모듈명을 명시 하지 않으면 글로벌 모듈을 모두 업데이트
```

### 모듈 삭제

```bash
npm uninstall 모듈명
# 로컬 모듈 삭제

npm uninstall 모듈명 -g
# 글로벌 모듈 삭제
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

이 경우 노드는 ./myModule/lib/temp.js 를 찾는다.
관련 내용을 설명한 블로그: http://nodejs.sideeffect.kr/docs/v0.10.7/api/modules.html#modules_folders_as_modules

## exports/require

노드에서 하나의 자바스크립트 파일은 하나의 모듈이 된다. 기본적으로 노드는 각 자바스크립트 파일을 익명함수로 감싸 외부에서 접근할 수 없게 만드는데 이것을 모듈화라고 하며 모듈간 참조와 호출을 위해 글로벌 객체 module의 exports/require를 사용한다.

함수나 변수는 `module.exports`에 할당하면 외부에서 접근할 수 있다(단, 명시적인 함수 이름이 있는 경우만).

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

module은 전역객체라서 객체명을 명시하지 않아도 된다.

### require()

모듈 불러오기.

```bash
require(경로/파일명);
```

경로는 문자열`' '`로 표시하며 경로를 생략할 경우 기본모듈이나 확장모듈(node.js가 기본적으로 제공하는 모듈 혹은 npm으로 설치한 모듈을 말하며 이 둘을 native module이라 한다.)을 불러온다.

같은 경로내에 불러올 js 파일이 있다면 경로는 `./`가 되고 한 단계 상위의 폴더라면 경로는 `../`가 된다.

```js
var userModule = require('./user_module');
```

`require()`로 불려진 파일은 노드 애플리케이션 내에 캐싱된다. 다시 말해 같은 파일을 여러번 호출해도 최초로 생성되었던 객체가 반복적으로 리턴된다. 별도의 인스턴스가 필요하다면 함수를 따로 호출하거나 functio을 리턴해 new로 인스턴스를 생성해서 사용한다.

객체의 프로퍼티로 내보내기:

```js
function printA(){};
var PI = 3.14;

exports.printA = printA;
exports.PI = PI;

// 위에서 내보낸 모듈은 다음처럼 불러온다
var userModule = require('[js를 제외한 파일 경로]');
userModule.printA();
console.log(userModule.PI);
```
