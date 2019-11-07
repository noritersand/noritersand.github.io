---
layout: post
date: 2016-09-08 18:47:28 +0900
title: '[Maven] 메이븐은 src/main/java 아래의 xml을 무시한다'
categories:
  - maven
tags:
  - buildtool
  - maven
  - pom.xml
---

#### 관련 문서

- [http://stackoverflow.com/questions/9798955/with-maven-clean-package-xml-source-files-are-not-included-in-classpath](http://stackoverflow.com/questions/9798955/with-maven-clean-package-xml-source-files-are-not-included-in-classpath)


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
