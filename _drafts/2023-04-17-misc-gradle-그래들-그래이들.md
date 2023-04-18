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


## 버전 관리 무시 대상으로 추가해야할 항목들

```
.gradle
gradle
gradlew
gradlew.bat
```


## 환경 변수 추가

TODO 이게 필요하던가? choco로 설치하면 필요 없을지도...

- GRADLE_HOME: `C:\dev\gradle` (gradle 설치경로)
- PATH: `%GRADLE_HOME%\bin`


## build.gradle

[https://docs.gradle.org/current/userguide/build_environment.html](https://docs.gradle.org/current/userguide/build_environment.html)

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

`build.gradle`은 Gradle에서 빌드를 구성하는 스크립트 파일이다. 메이븐의 `pom.xml`과 비슷하다.


## settings.gradle

```bash
rootProject.name = 'gradle-example'
```

TODO


## gradle.properties

TODO


## 메이븐 프로젝트에서 전환하기

[https://docs.gradle.org/8.1/userguide/migrating_from_maven.html](https://docs.gradle.org/8.1/userguide/migrating_from_maven.html)

기존 프로젝트의 루트에서 `gradle init`을 실행하면 메이븐 빌드를 발견했다며, 이를 바탕으로 그래들 빌드 설정을 만들지 묻는다:

```bash
> gradle init

Welcome to Gradle 8.1!

Here are the highlights of this release:
 - Stable configuration cache
 - Experimental Kotlin DSL assignment syntax
 - Building with Java 20

For more details see https://docs.gradle.org/8.1/release-notes.html

Starting a Gradle Daemon (subsequent builds will be faster)

Found a Maven build. Generate a Gradle build from this? (default: yes) [yes, no] yes

...

> Task :init
Maven to Gradle conversion is an incubating feature.
Get more help with your project: https://docs.gradle.org/8.1/userguide/migrating_from_maven.html

BUILD SUCCESSFUL in 58s
2 actionable tasks: 2 executed
```

`yes`를 입력해주고 진행하면:

- `build.gradle`
- `settings.gradle` 
- `.gradle`
- `gradle`
- `gradlew`
- `gradlew.bat`

이 생성된다.


## 빌드하기

