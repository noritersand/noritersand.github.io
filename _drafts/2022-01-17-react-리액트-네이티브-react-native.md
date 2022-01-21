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

그리고 **WSL에서 진행**함.

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

### 안드로이드 스튜디오

- [Android Studio](https://developer.android.com/studio)
- [Android Studio: User guide](https://developer.android.com/studio/intro)

일단 설치

#### SDK

안드로이드 스튜디오에서 Android SDK 설정으로 이동한다: Settings<kbd>ctrl + alt + s</kbd> > Appearance & Behavior > System Settings > Android SDK

그리고 원하는 버전 아무거나 골라서 설치, 이 때 'Show Package Details' 눌러서:

- Intel x86 Atom_64 System Image
- Google APIs Intel x86 Atom_64 System Image
- Google Play Intel x86 Atom_64 System Image

선택 후 진행하면 됨.

TODO Package 설명 추가

#### ADV<sup>Android Virtual Device</sup>

ADV는 안드로이드를 에뮬레이팅할 때 사용할 OS 기기의 구성 정도로 이해하면 된다.

역시 적당한 걸로 아무거나 고른다.

#### 환경 변수, 별칭 추가

WSL의 환경 변수를 아래처럼 추가한다:

```bash
export LOCAL_USERHOME="/mnt/c/Users/fixal"
export ANDROID_HOME="$LOCAL_USERHOME/AppData/Local/Android/Sdk"
export PATH="$PATH:$ANDROID_HOME/emulator"
export PATH="$PATH:$ANDROID_HOME/tools"
export PATH="$PATH:$ANDROID_HOME/tools/bin"
export PATH="$PATH:$ANDROID_HOME/platform-tools"
```

여기서 `ANDROID_HOME`의 경로는 안드로이드 스튜디오의 Android SDK Location의 값과 일치해야 함.

그리고:

```bash
alias adb='adb.exe'
```

추가한 다음 `adb` 명령어 잘 실행되는지 확인.

#### Java

WSL에 자바 설치

```bash
apt install openjdk-17-jdk
```
