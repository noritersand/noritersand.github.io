---
layout: post
date: 2022-01-17 18:49:33 +0900
title: '[React Native] 리액트 네이티브 개발 환경 구성'
categories:
  - react-native
tags:
  - react
  - react-native
  - javascript
  - environment
  - settings
---

* Kramdown table of contents
{:toc .toc}

{% raw %}

#### 참고 문서

- [React Native \| Introduction](https://reactnative.dev/docs/getting-started)
- [React Native \| APIs](https://reactnative.dev/docs/accessibilityinfo)
- [React Native \| Setting up the development environment](https://reactnative.dev/docs/environment-setup)

#### 테스트 환경 정보

- 2022-01-24 작성
- Windows 11 (21H2)
- PowerShell 7.x
- NVM 1.1.9
- Node.js 17.4.0
- npm/npx 8.3.1
- React Native CLI 2.0.1


## 개요

리액트 네이티브 개발 환경 구성 방법 요약 정리. 글쓴이는 맥을 안써서 iOS는 해당 엇ㅂ음.


## 설치

React Native CLI로 진행함. 잘 알려진 지원 툴(프레임워크인가?)로 Expo CLI가 있는데, 이 글에서 따로 언급하지 않음.

### NVM, Node.js, npm

NVM은 여러 버전의 Node.js를 설치하고 선택할 수 있게 하는 툴임.

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

npm은 Node.js와 함께 설치된다.

### 안드로이드 스튜디오

- [Android Studio](https://developer.android.com/studio)
- [Android Studio: User guide](https://developer.android.com/studio/intro)

설치하고 `react-native init`으로 생성한 경로 아래에 `android` 디렉터리를 프로젝트로 열기.

#### Gradle

Gradle 빌드는 JDK도 필요한데, JDK 버전이 최신이면 Gradle의 버전도 올려줘야 한다고 함.

`{project root folder}\android\gradle\wrapper\gradle-wrapper.properties` 파일에서 `distributionUrl` 항목을 최신 버전으로 수정한다. Gradle의 릴리즈 정보는 [여기](https://gradle.org/releases/)에서 확인.

예시:

```bash
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3.3-all.zip
```

그냥 안드로이드 스튜디오 > Project Structure(<kbd>ctrl + alt + shift + s</kbd>)에서 바꿔도 된다.

#### SDK Platforms

안드로이드 스튜디오에서 Android SDK 설정으로 이동한다: Settings(<kbd>ctrl + alt + s</kbd>) > Appearance & Behavior > System Settings > Android SDK > SDK Platforms

그리고 원하는 버전 아무거나 골라서 설치하면 되는데, 리액트 네이티브를 빌드하려면 Android 10 (Q)는 필수라고 함.

우측 하단의 'Show Package Details' 눌러서 Android 10 (Q) 아래에 있는:

- Android SDK Platform 29
- Intel x86 Atom_64 System Image 혹은 Google APIs Intel x86 Atom System Image

요렇게 선택 후 (원하면 추가로 고르고) 진행.

#### SDK Tools

당장 손댈 것은 없고, 나중에 "Installed Build Tools revision x.x.x is corrupted. Remove and install again using the SDK Manager." 라는 메시지와 함께 빌드가 안 되면 해당 버전의 빌드 툴을 찾아 재설치하면 된다.

#### 환경 변수 설정

그 다음 설치된 SDK 경로를 환경 변수 `ANDROID_HOME`으로 추가함. 기본값은 `%LOCALAPPDATA%\Android\Sdk`라고 한다.

```bash
[Environment]::SetEnvironmentVariable("ANDROID_HOME", "$env:LOCALAPPDATA\Android\Sdk", "User")
```

PATH에 요것도 추가:

```bash
[Environment]::SetEnvironmentVariable("PATH", "$env:PATH;$env:LOCALAPPDATA\Android\Sdk\platform-tools", "User")
```

파워셸 재시작 후 `adb` 명령어 잘 실행되는지 확인.

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
react-native init NEW_DIR

# --version: 특정 리액트 네이티브의 버전 지정
react-native init NEW_DIR --version X.XX.X

# --template: 래익트 네이티브 템플릿 지정 옵션
react-native init NEW_DIR --template react-native-template-typescript
```


## 앱 시작하기

### Metro

일단 앱을 시작하기 전에 메트로를 띄워놔야 한다. (메트로는 웹팩하고 비슷한 번들러라고 함)

```bash
# npm start
react-native start
```

이 과정에서 만약 다음과 같은 에러가 발생하면:

```bash
Failed to construct transformer:  Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    ... 생략
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  ... 생략
```

[OpenSSL 관련 버전이 안맞아서](https://stackoverflow.com/questions/69665222/node-js-17-0-1-gatsby-error-digital-envelope-routinesunsupported-err-os) 그런건데, **Node.js 버전을 16 이하로 내리거나** 옵션으로 `--openssl-legacy-provider`를 지정해줘야 함.

`package.json`을 이렇게 수정:

```bash
"scripts": {
  "start": "SET NODE_OPTIONS=--openssl-legacy-provider && react-native start"
},
```

이후 메트로는 항상 npm 스크립트로 시작.

### 디바이스 설정

앱을 실행할 디바이스를 결정한다. 실기기인 경우 그냥 케이블로 연결하고 (개발자 모드로 변환하고, 연결 허용도 하고...) 다음으로 진행하면 된다.

만약 가상 디바이스가 필요하면 아래를 볼 것.

### AVD Manager

AVD(Android Virtual Device)는 안드로이드를 에뮬레이팅할 때 사용할 OS 기기의 구성 정도로 이해하면 된다. 버튼의 위치는 안드로이드 스튜디오의 버전에 따라 다를 수 있다. 2020.3.1 버전은 우측 상단에 있음.

역시 적당한 걸로 아무거나 골라서 추가한다. 그리고 `Launch this AVD in the emulator` 클릭(재생 버튼).

### Gradle build

안드로이드는 `react-native`로 앱을 실행하기 전에 Gradle 빌드를 한 번 해줘야 한다. 안드로이드 스튜디오에서 'Sync Project with Gradle Files' 해주면 됨.

만약 빌드 중 `build.gradle` 파일에서:

```
import com.android.build.OutputFile
```

이 부분이 문제가 있다면 해당 라인만 코멘트 처리하고 'Sync Project with Gradle Files' 한 번 (이 때 에러 발생하는데 무시하고), 코멘트 다시 지우고 Sync 다시 하면 잘 될 수도(?) 있음😵‍💫

### 앱 실행

메트로를 실행중인 터미널은 그대로 둔다.

이제 앱 실행 방법은 두 가지인데:

#### React Native CLI로 앱 실행

새 터미널을 열고 아래 입력:

```bash
# npm run android
react-native run-android
```

하면 빌드 후 연결된 디바이스에서 앱을 실행 한다. 만약 메트로가 안떠있으면 스스로 띄운다.

#### 안드로이드 스튜디오에서 Run 'app'

단축키는 <kbd>shift + f10</kbd>

실행하면 "Unable to load script. Make sure you're either running Metro (run 'npx react-native start') or that your bundle 'index.android.bundle' is packaged correctly for release." 에러가 발생할 수 있다. 검색해보면 여러 조치 방법이 나오는데, 무시하고 개발자 메뉴(디바이스 흔들기)의 'Change Bundle Location'에서 PC의 `네트워크_IP:8081`를 입력하면 해결되긴 함. 만약 이렇게 했을 때 응답이 없으면 안드로이드 스튜디오에서 Run 다시 해주면 됨.


## 디버깅

리액트 네이티브에 기본 내장된 웹 버전 디버거와 앱 버전 디버거가 있다. 일단 연결되면 마치 브라우저 개발자 도구 쓰듯이 쓸 수 있다. (콘솔, 노드 트리, 코드 브레이킹, 네트워크 트래픽 등등)

AVD에선 잘 되는데 실 기기로 테스트 했을 때는 연결이 안 되거나 브레이크가 제대로 안걸리거나 하는 문제가 있었음.

### 웹 버전 디버거 Chrome Developer Tools

디버그 모드 상태에서 크롬으로 [http://localhost:8081/debugger-ui](http://localhost:8081/debugger-ui)에 접속하면 된다.

디버그 모드는 메트로에서 <kbd>d</kbd>를 눌러 developer menu를 열고 'Debug' 혹은 'Debug JS Remotely'를 선택해 활성화한다. 요러면 기본 브라우저에서 자동으로 디버거 페이지에 접속됨.

### React Native Debugger

[다운로드 페이지](https://github.com/jhen0409/react-native-debugger/releases)

[크로뮴](https://www.chromium.org) 기반 독립 앱이다.

연결 방법은:

1. 디버거 실행
2. 메트로 실행
3. 앱 실행
4. 디버그 모드 활성화
5. 끗

메트로&앱을 디버거보다 먼저 실행했으면 <kbd>r</kbd>을 눌러 새로고침 해줘야 함.

근데 디버거 사용성이 좀 션찮다... 게다가 리프레시도 느려짐. (윈도우만 그런가?)

### [react-native-reanimated](https://docs.swmansion.com/react-native-reanimated/)를 쓰는 경우 리모트 디버깅 불가능

2022-02-15 확인:

> As the library uses JSI for synchronous native methods access, remote debugging is no longer possible. You can use Flipper for debugging your JS code, however connecting debugger to JS context which runs on the UI > thread is not currently supported.
>
> [https://docs.swmansion.com/react-native-reanimated/docs/#library-overview](https://docs.swmansion.com/react-native-reanimated/docs/#library-overview)


{% endraw %}
