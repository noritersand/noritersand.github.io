---
layout: post
date: 2022-01-17 18:49:33 +0900
title: '[React] 리액트 네이티브 개발 환경 설정 React Native settings'
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
- [\[React Native\] Setting up the development environment](https://reactnative.dev/docs/environment-setup)

#### 버전 정보

- 2022-01-24 작성
- Windows 11 (21H2 build 22000.434)
- PowerShell 7.x
- NVM 1.1.9
- Node.js 17.4.0
- NPM/NPX 8.3.1
- React Native CLI 2.0.1

## 개요

리액트 네이티브 개발 환경 구성 방법 요약 정리. 글쓴이는 맥을 안써서 iOS는 해당 엇ㅂ음.

## 설치

개발 환경을 구성하는 방법은 크게 Expo CLI가 있고 React Native CLI가 있는데, Expo CLI는 이 글에서 따로 언급하지 않음.

### NVM, Node.js, NPM

NVM은 여러 버전의 Node.js를 설치하고 선택할 수 있게 하는 툴이다.

```bash
# PowerShell에서
choco install nvm
```

```bash
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

설치하고 `react-native init`으로 생성한 경로 아래에 `android` 디렉터리를 프로젝트로 열기.

#### Gradle

리액트 빌드는 JDK도 필요한데, JDK 버전이 최신이면 Gradle의 버전도 올려줘야 한다고 함.

`{project root folder}\android\gradle\wrapper\gradle-wrapper.properties` 파일에서 `distributionUrl` 항목을 최신 버전으로 수정한다. Gradle의 릴리즈 정보는 [여기](https://gradle.org/releases/)에서 확인.

예시:

```bash
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3.3-all.zip
```

그냥 안드로이드 스튜디오 > Project Structure<kbd>ctrl + alt + shift + s</kbd>에서 바꿔도 된다.

#### SDK Platforms

안드로이드 스튜디오에서 Android SDK 설정으로 이동한다: Settings<kbd>ctrl + alt + s</kbd> > Appearance & Behavior > System Settings > Android SDK > SDK Platforms

그리고 원하는 버전 아무거나 골라서 설치하면 되는데, 리액트 네이티브를 빌드하려면 Android 10 (Q)는 필수라고 함.

우측 하단의 'Show Package Details' 눌러서 Android 10 (Q) 아래에 있는:

- Android SDK Platform 29
- Intel x86 Atom_64 System Image 혹은 Google APIs Intel x86 Atom System Image

요렇게 선택 후 (원하면 추가로 고르고) 진행.

#### SDK Tools

당장은 딱히 손댈 것은 없고, 나중에 "Installed Build Tools revision x.x.x is corrupted. Remove and install again using the SDK Manager." 라는 메시지와 함께 빌드가 안되면 해당 버전의 빌드 툴을 찾아 재설치하면 된다.

#### 환경 변수 설정

그 다음 설치된 SDK 경로를 환경 변수 `ANDROID_HOME`으로 추가함. 기본값은 `%LOCALAPPDATA%\Android\Sdk`라고 한다.

```bash
[Environment]::SetEnvironmentVariable("ANDROID_HOME", "$env:LOCALAPPDATA\Android\Sdk", "User")
```

PATH에 요것도 추가:

```bash
[Environment]::SetEnvironmentVariable("PATH", "$env:PATH;$env:LOCALAPPDATA\Android\Sdk\platform-tools", "User")
```

파워쉘 재시작 후 `adb` 명령어 잘 실행되는지 확인.

### React Native CLI

```bash
npm install react-native-cli -g
npm install yarn -g # Yarn이 있으면 init 할 때 빠름

# ... 생략

#  Run instructions for Android:
#    • Have an Android emulator running (quickest way to get started), or a device connected.
#    • cd "/home/fixalot/repo/testbed-reactnative/tutorial" && npx react-native run-android
#
#  Run instructions for Windows:
#    • See https://aka.ms/ReactNativeGuideWindows for the latest up-to-date instructions.
```

```bash
# NEW_DIR 경로에 새 리액트 네이티브 앱 구성파일 생성
npx react-native init NEW_DIR

# --version: 특정 리액트 네이티브의 버전 지정
npx react-native init NEW_DIR --version X.XX.X

# --template: 래익트 네이티브 템플릿 지정 옵션
npx react-native init NEW_DIR --template react-native-template-typescript
```

## 앱 시작하기

### Metro

일단 앱을 시작하기 전에 메트로를 띄워놔야 한다. (메트로는 웹팩하고 비슷한 번들러라고 함)

```bash
# npx react-native start
npm start
```

이 과정에서 만약 다음과 같은 에러가 발생하면:

```bash
Failed to construct transformer:  Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    ... 생략
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  ... 생략
```

[OpenSSL 관련 버전이 안맞아서](https://stackoverflow.com/questions/69665222/node-js-17-0-1-gatsby-error-digital-envelope-routinesunsupported-err-os) 그런건데, 이 경우 Node.js를 16으로 내리거나 node 실행 옵션으로 `--openssl-legacy-provider`를 지정해줘야 함.

`package.json`을 이렇게 수정:

```bash
"scripts": {
  "start": "SET NODE_OPTIONS=--openssl-legacy-provider && react-native start"
},
```

이후 메트로는 항상 NPM 스크립트로 시작.

### 디바이스 설정

앱을 실행할 디바이스를 결정한다. 실기기인 경우 그냥 케이블로 연결하고 (개발자모드로 변환하고, 연결 허용도 하고...) 다음으로 진행하면 된다.

만약 가상 디바이스가 필요하면 저 아래 [AVD Manager](##heading-AVD-Manager)를 볼 것.

### 앱 배포와 실행

매트로를 실행중인 터미널은 그대로 둔다. 새 터미널을 열고 아래 입력:

```bash
npm run android
# node npx react-native run-android
```

하면 소스 빌드 후 연결된 디바이스에 배포/실행 한다. 만약 메트로가 안떠있으면 스스로 띄운다.

만약 빌드 중 `build.gradle` 파일에서

```
import com.android.build.OutputFile
```

이 부분이 문제가 있다면 해당 라인만 코멘트 처리하고 'Sync Project with Gradle Files' 한 번 (이 때 에러 발생하는데 무시하고), 코멘트 다시 지우고 Sync 다시 하면 잘 될 수도(?) 있음;;

### AVD Manager

AVD<sup>Android Virtual Device</sup>는 안드로이드를 에뮬레이팅할 때 사용할 OS 기기의 구성 정도로 이해하면 된다. 버튼의 위치는 안드로이드 스튜디오의 버전에 따라 다를 수 있다. 2020.3.1 버전은 우측 상단에 있음.

역시 적당한 걸로 아무거나 골라서 추가한다. 그리고 `Launch this AVD in the emulator` 클릭(재생 버튼).

그 다음 안드로이드 스튜디오 UI 어딘가에 있는 배포 대상 디바이스를 선택하고(이 글 작성 시점엔 1시 방향에 있음)

이제 매트로 실행 후 앱 시작하면 됨.

```bash
npm start
# 새 터미널 열어서
npm run android
```
