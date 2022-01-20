---
layout: post
date: 2022-01-17 18:49:33 +0900
title: '[React] 리액트 네이티브 React Native'
categories:
  - react
tags:
  - react
  - react-native
  - javascript
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[React Native\] Introduction](https://reactnative.dev/docs/getting-started)
- [\[React Native\] APIs](https://reactnative.dev/docs/accessibilityinfo)

## 제목

내용

끗.

## 설치

Expo CLI가 있고 React Native CLI가 있는데, Expo CLI는 편하긴 한디 단점이 너무 많아서 따로 언급하지 않음.

이 글에선 번거롭지만 빌드 환경을 직접 구성하는 React Native CLI로 한다.

### NVM, Node.js, NPM

NVM은 여러 버전의 Node.js를 설치하고 선택할 수 있게 하는 툴이다.

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

# 최신버전으로 node 설치
nvm install node

# 특정 버전으로 node 설치
nvm install 10.15.1

# 설치된 node 버전 목록과 상태를 표시
nvm ls

# node 10.15.1 버전 활성화
nvm use 10.15.1

# node 버전 확인
node --version
```

NPM은 Node.js와 함께 설치된다.
