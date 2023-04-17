---
layout: post
date: 2017-06-09 19:12:00 +0900
title: '[Maven] POM, pom.xml'
categories:
  - maven
tags:
  - build-tool
  - maven
  - pom
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Maven: Settings Reference](https://maven.apache.org/settings.html)
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


## exclusion을 추가해도 당장 컴파일 에러가 발생하지 않는 이유

메이븐 디펜던시로 추가한 라이브러리들은 이미 컴파일이 끝난 상태로 사용되기 때문에 해당 라이브러리에서 의존하는 라이브러리를 제외시켰다고 해서 당장 컴파일 에러가 발생하지는 않는다. 대신 RuntimeException이 발생한다. 하지만 제외시킨 라이브러리를 사용하지만 않으면 아무 문제가 없다.


## 프로젝트 정보

- modelVersion:
- groupId:
- artifactId:
- version:
- packaging:
- name:
- url:

## 프로퍼티

- properties
  - encoding
  - project.build.sourceEncoding
  - project.reporting.outputEncoding
  - java-version

## 저장소 설정

라이브러리나 플러그인을 찾을 저장소를 지정한다. 메이븐 센트럴에 없으면 여기서 추가한 저장소에서 찾는다.

- repositories
- distributionManagement

## 의존관계 설정

`<dependencyManagement>` 혹은 `<dependencies>`로 시작한다.

- dependencyManagement
  - dependencies
    - dependency
      - groupId
      - artifactId
      - version
      - optional: 이 항목이 true인 경우 이 모듈을 의존하는 다른 모듈에 포함시키지 않음.
      - classifier
      - exclusions
      - type
      - scope
      - systemPath

### classifier

정식 명칭은 Maven artifact classifier이며 동일한 버전의 빌드에서 내용이 다른 아티팩트를 지정하는 용도로 사용한다. 버전 번호 바로 앞에 붙는다.

```
org.maven:example-lib:this-is-classifier:1.0.0
```

만들고 싶으면 빌드 플러그인에서 configuration - classifier를 명시하면 됨.

```xml
<!-- 소스 출처: https://www.baeldung.com/maven-artifact-classifiers#1-generating-artifacts-with-a-classifier -->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.2</version>
            <executions>
                <execution>
                    <id>Arbitrary</id>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                    <configuration>
                        <classifier>arbitrary</classifier>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

실제 사례로 netty는 M2 맥에서 네이티브 라이브러리가 없다는 UnsatisfiedDependencyException을 발생시키는데, 이 문제는 아래처럼:

```xml
<dependency>
  <groupId>io.netty</groupId>
  <artifactId>netty-resolver-dns-native-macos</artifactId>
  <version>4.1.87.Final</version>
  <classifier>osx-aarch_64</classifier>
</dependency>
```

M2용 라이브러리를 지정하는 방법으로 해결할 수 있다.

이걸 `mvn dependency:tree`로 비교해보면:

```
[INFO] \- io.netty:netty-resolver-dns-native-macos:jar:4.1.79.Final:compile
[INFO]    \- io.netty:netty-resolver-dns-classes-macos:jar:4.1.79.Final:compile
[INFO]       +- io.netty:netty-resolver-dns:jar:4.1.79.Final:compile
[INFO]       |  \- io.netty:netty-codec-dns:jar:4.1.79.Final:compile
[INFO]       \- io.netty:netty-transport-native-unix-common:jar:4.1.79.Final:compile
```

```
[INFO] \- io.netty:netty-resolver-dns-native-macos:jar:osx-aarch_64:4.1.79.Final:compile
[INFO]    \- io.netty:netty-resolver-dns-classes-macos:jar:4.1.79.Final:compile
[INFO]       +- io.netty:netty-resolver-dns:jar:4.1.79.Final:compile
[INFO]       |  \- io.netty:netty-codec-dns:jar:4.1.79.Final:compile
[INFO]       \- io.netty:netty-transport-native-unix-common:jar:4.1.79.Final:compile
```

요롷게 다름.


### scope

해당 라이브러리가 어느 시점에 사용되는지 제한하는 설정이다. 다섯 가지가 있다.

- compile: 기본값. 컴파일, 런타임, 테스트 시 모두 사용
- provided
- runtime
- system
- test: 테스트 때만 사용한다.

## 빌드 설정

- build
  - finalName: 프로젝트를 빌드할 때 사용할 이름이다. m2e\*에서 이 값은 이클립스 설정 중 `Project Properties > Web Project Settings > Context root`를 덮어쓴다.
    - plugins
      - plugin
        - artifactId
        - version
        - configuration

\* m2e: 이클립스의 메이븐 지원 플러그인


## 플러그인

메이븐 플러그인은 메이븐 명령 실행 시 먼저 수행하거나 나중에 수행 할 일, 그리고 명령의 세부 조정이 필요할 때 사용한다. (가령 어떤 파일의 위치를 메이븐에 알린다거나 특정 위치의 파일들을 복사해온다던지... 등등)

아래처럼 작성한다.:

```xml
<build>
  <plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.2</version>
        <configuration>
            <webResources>
                <resource>
                    <!-- this is relative to the pom.xml directory -->
                    <directory>src/main/webapp</directory>
                </resource>
            </webResources>
        </configuration>
    </plugin>
  </plugins>
</build>
```

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-resources-plugin</artifactId>
      <executions>
        <execution>
          <id>copy-resources</id>
          <phase>process-classes</phase>
          <goals>
            <goal>copy-resources</goal>
          </goals>
          <configuration>
            <outputDirectory>${basedir}/target/classes</outputDirectory>
            <encoding>UTF-8</encoding>
            <resources>
              <resource>
                <directory>${rootPath}/src/main/resources</directory>
                <includes>
                  <include>**/*.*</include>
                </includes>
              </resource>
            </resources>
          </configuration>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

현재(2022-06-13)까지는 빌드 플러그인과 리포팅 플러그인 딱 두 종류만 있음.

### maven-compiler-plugin

컴파일러 플러그인

#### configurations

- source
- target
- encoding

### maven-war-plugin

WAR 빌드용 플러그인

#### configurations

- warSourceDirectory
- warSourceExcludes
- webappDirectory
- webXml
- webResources
  - webResource: 하위 규칙에 따라 특정 경로의 파일들을 ROOT 경로로 내보낸다.
    - directory: 내보낼 대상을 디렉터리로 지정 (e.g. `src/main/resources/static`)


## 프로파일

로컬 환경에 따라 달라지는 빌드를 구성할 때 사용한다. 가령 특정 프로세서를 사용하는 환경일 땐 필요한 추가 라이브러리를 다운로드한다던지...

예를 들어 아래는 M1 프로세서를 사용하는 macOS일 때 `io.netty:netty-resolver-dns-native-macos:osx-aarch_64`를 추가하는 설정이다:

```xml
<profile>
    <id>macos-m1</id>
    <activation>
        <os>
            <family>mac</family>
            <arch>aarch64</arch>
        </os>
    </activation>
    <dependencies>
        <dependency>
            <groupId>io.netty</groupId>
            <artifactId>netty-resolver-dns-native-macos</artifactId>
            <classifier>osx-aarch_64</classifier>
        </dependency>
    </dependencies>
</profile>
```

- <profiles>
  - <profile>

