---
layout: post
date: 2022-01-27 13:15:42 +0900
title: '[React] 리액트 네이티브 노트'
categories:
  - react
tags:
  - react
  - react-native
  - javascript
  - note
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[React Native\] APIs](https://reactnative.dev/docs/accessibilityinfo)
- [Core Components and APIs](https://reactnative.dev/docs/components-and-apis)

#### 버전 정보

- Windows 11 (21H2)
- PowerShell 7.x
- NVM 1.1.9
- Node.js 16.13.0
- NPM/NPX 8.3.1
- React Native CLI 2.0.1


## 개요

리액트 네이티브 개발 환경 구성 방법 요약 정리. 글쓴이는 맥을 안써서 iOS는 해당 엇ㅂ음.


## Core Components

### [View](https://reactnative.dev/docs/view)

TODO


## 이벤트

### onPress

TODO


## 그 외 모든 것

### 기기의 뒤로 가기 버튼에 페이지 변환되지 않도록 방지 처리

`<Screen/>`일 때는 `BackHandler`를:

```js
import React, {useEffect} from "react";
import {BackHandler} from "react-native";

// ...

useEffect(() => {
  const onBackPress = () => {
    cancel();
    return true;
  };
  BackHandler.addEventListener("hardwareBackPress", onBackPress);
  return () =>
    BackHandler.removeEventListener("hardwareBackPress", onBackPress);
}, []);
```

`<Modal/>`일 땐 `onBackButtonPress` 쓰면 됨:

```js
<Modal
  onBackButtonPress={ok}
>
```
