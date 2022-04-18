---
layout: post
date: 2017-06-09 19:12:00 +0900
title: '[Maven] POM, pom.xml'
categories:
  - maven
tags:
  - buildtool
  - maven
  - pom
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Maven: POM Reference](https://maven.apache.org/pom.html#What_is_the_POM)
- [Maven: Available Plugins](https://maven.apache.org/plugins/index.html)

#### 버전 정보

- maven 4.0.0

## 개요

POM은 'Project Object Model'의 약자로 메이븐 프로젝트의 설정과 설명 등을 기술하는 XML 파일을 말한다. 보통은 pom.xml로 작성한다.

```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- The Basics -->
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <packaging>...</packaging>
    <dependencies>...</dependencies>
    <parent>...</parent>
    <dependencyManagement>...</dependencyManagement>
    <modules>...</modules>
    <properties>...</properties>

    <!-- Build Settings -->
    <build>...</build>
    <reporting>...</reporting>

    <!-- More Project Information -->
    <name>...</name>
    <description>...</description>
    <url>...</url>
    <inceptionYear>...</inceptionYear>
    <licenses>...</licenses>
    <organization>...</organization>
    <developers>...</developers>
    <contributors>...</contributors>

    <!-- Environment Settings -->
    <issueManagement>...</issueManagement>
    <ciManagement>...</ciManagement>
    <mailingLists>...</mailingLists>
    <scm>...</scm>
    <prerequisites>...</prerequisites>
    <repositories>...</repositories>
    <pluginRepositories>...</pluginRepositories>
    <distributionManagement>...</distributionManagement>
    <profiles>...</profiles>
  </project>
```

## pom.xml의 구성요소

### 프로젝트 정보

- modelVersion:
- groupId:
- artifactId:
- version:
- packaging:
- name:
- url:

### 프로퍼티

- properties
  - encoding
  - project.build.sourceEncoding
  - project.reporting.outputEncoding
  - java-version

### 메이븐 저장소

- repositories
- distributionManagement

### 의존관계 설정

- dependencies
- dependencyManagement

### 빌드 설정

- build
  - finalName: 프로젝트를 빌드할 때 사용할 이름이다. m2e\*에서 이 값은 이클립스 설정 중 `Project Properties` > `Web Project Settings` > `Context root`를 덮어쓴다.
    - plugins
      - plugin
        - artifactId
        - version
        - configuration

\* m2e: 이클립스의 메이븐 지원 플러그인

#### [플러그인: maven-compiler-plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)

- source
- target
- encoding

#### [플러그인: maven-war-plugin](https://maven.apache.org/plugins/maven-war-plugin/)

- warSourceDirectory
- warSourceExcludes
- webXml
- webResources
  - webResource: 하위 규칙에 따라 특정 경로의 파일들을 `target\m2e-wtp\web-resources`(default)* 경로로 내보낸다.
    - directory: 내보낼 대상의 경로를 디렉터리로 지정 (e.g. `src/main/webroot`)

* `target\m2e-wtp\web-resources`는 WAR로 배포할 때 루트 경로로 내보내짐.
