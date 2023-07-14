---
layout: post
date: 2018-02-22 13:58:28 +0900
title: '[Maven] 아파치 메이븐 노트'
categories:
  - maven
tags:
  - build-tool
  - maven
  - nexus
  - link-post
---

* Kramdown table of contents
{:toc .toc}


## 개요

ㅁㄴㅇㅎㅂㅈㄱ


## Help 플러그인

### 설정 확인

```
mvn help:effective-pom
```

현재 빌드의 유효한 POM을 XML로 표시한다. 프로젝트에 모든 POM 정보를 고려한 최종 POM 구성을 확인할 수 있다. 쉽게 `pom.xml` 파일에서 유효한(적용중인) 설정을 표시하는 기능이다.

```
mvn help:effective-settings
```

프로젝트의 계산된 설정(?)을 XML로 표시한다. 로컬/원격 저장소, 미러, 프록시, 인증 정보 등을 포함한다. 이 쪽은 `settings.xml` 파일의 내용을 표시한다.


## dependency 플러그인 

### 의존관계 트리 보기

```
mvn dependency:tree
```

### 소스 코드 다운로드

```
mvn dependency:sources
```

사실 공식 문서의 설명은 "tells Maven to resolve all dependencies and their source attachments, and displays the version" 인데 어쨋든 소스 받는거임 😏


## 메이븐은 src/main/java 아래의 xml을 무시한다

[http://stackoverflow.com/questions/9798955/with-maven-clean-package-xml-source-files-are-not-included-in-classpath](http://stackoverflow.com/questions/9798955/with-maven-clean-package-xml-source-files-are-not-included-in-classpath)


정확하게 말하면 `src/main/java` 이하의 폴더에 존재하는 java 이외의 파일을 모두 무시한다. 이 때문에 이클립스 빌드땐 정상이지만 메이븐 빌드 후엔 특정 파일들이 누락되면서 런타임 오류가 발생할 수 있다.

이를 해결하려면 다음 둘 중 하나의 방법을 따른다:

- java 이외의 파일은 `src/main/resources` 아래에 위치시킨다.
- 특정 확장자를 포함하도록 pom 파일을 수정한다.

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
        </resource>
    </resources>
</build>
```

참고로 `maven-war-plugin`은 해당사항이 없는데, war로 패키징 할 땐 `src/main/java`에 있는 java 이외의 파일이라도 모두 묶어서 WEB-INF 아래에 위치하게 한다.


## 넥서스 설치

프로 버전이 나오면서 다운로드 링크 찾기가 힘들어졌는데, 'nexus oss download'로 검색하면 바로 나오니 참고할 것.

- [https://help.sonatype.com/repomanager3/product-information/download](https://help.sonatype.com/repomanager3/product-information/download)
- [https://help.sonatype.com/repomanager3/product-information/download/download-archives---repository-manager-3](https://help.sonatype.com/repomanager3/product-information/download/download-archives---repository-manager-3)
- [https://help.sonatype.com/repomanager2/download](https://help.sonatype.com/repomanager2/download)


[https://support.sonatype.com/hc/en-us/articles/213465818-How-can-I-programmatically-upload-an-artifact-into-Nexus-2-](https://support.sonatype.com/hc/en-us/articles/213465818-How-can-I-programmatically-upload-an-artifact-into-Nexus-2-)


## descriptor

TODO 이거 뭐냐

#### pom.xml

```xml
<plugin>
    <!-- assamble static content -->
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
        <descriptors>
            <descriptor>src/main/resources/static.xml</descriptor>
        </descriptors>
    </configuration>
    <executions>
        <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

#### static.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<assembly>
    <id>static</id>
    <formats>
        <format>zip</format>
    </formats>
    <includeBaseDirectory>false</includeBaseDirectory>
    <fileSets>
        <fileSet>
            <directory>src/main/webapp</directory>
            <includes>
                <include>/api/**/*</include>
                <include>/css/**/*</include>
                <include>/error/**/*</include>
                <include>/guide/**/*</include>
                <include>/img/**/*</include>
                <include>/js/**/*</include>
                <include>/pub/**/*</include>
                <include>/template/**/*</include>
            </includes>
            <excludes>
                <exclude>WEB-INF/*</exclude>
            </excludes>
            <outputDirectory></outputDirectory>
        </fileSet>
    </fileSets>
</assembly>
```

뭐하는놈인거시냐
