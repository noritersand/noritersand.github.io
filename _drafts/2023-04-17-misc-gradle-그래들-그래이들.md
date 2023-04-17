---
layout: post
date: 2023-04-17 14:38:25 +0900
title: '[misc] Gradle'
categories:
  - misc
tags:
  - misc
  - gradle
  - build-tool
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://docs.gradle.org](https://docs.gradle.org)
- [https://www.slipp.net/wiki/pages/viewpage.action?pageId=11632703](https://www.slipp.net/wiki/pages/viewpage.action?pageId=11632703)
- [http://kwonnam.pe.kr/wiki/gradle](http://kwonnam.pe.kr/wiki/gradle)


#### 버전 정보

- 8.1 기준으로 작성됨


## 개요

우리말로 그래이들, 그래들 정도로 표기하는 Gradle은 소프트웨어 패키지 또는 프로젝트의 구축, 테스트, 배포 자동화 툴이다. 쉽게 차세대(라고 지금 부르기엔 출시한지 한참 됐지만) 메이븐이라고 생각하면 된다. 개발 언어는 Groovy.


## 설치

설치는 [여기](https://gradle.org/releases/)에서 다운로드 받거나, [Chocolatey](https://community.chocolatey.org/packages?q=gradle)로 설치한다.

```bash
choco install gradle
```


## 환경 변수 추가

TODO 이게 필요하던가? choco로 설치하면 필요 없을지도...

- GRADLE_HOME: `C:\dev\gradle` (gradle 설치경로)
- PATH: `%GRADLE_HOME%\bin`


## build.gradle

[https://docs.gradle.org/current/userguide/build_environment.html](https://docs.gradle.org/current/userguide/build_environment.html)

`build.gradle`은 Gradle에서 빌드를 구성하는 스크립트 파일이다. 메이븐의 `pom.xml`과 비슷하다.

### 작성 예시

```bash
apply plugin: 'java'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'com.google.guava:guava:30.1.1-jre'
    testImplementation 'junit:junit:4.13.2'
}
```

