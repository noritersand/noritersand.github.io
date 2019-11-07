---
layout: post
date: 2017-04-24 10:49:00 +0900
title: '[Tomcat] webapp 이름이 ROOT가 아니며 path가 "/"일 때'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - appbase
  - docbase
  - webapp
  - webroot
---

* Kramdown table of contents
{:toc .toc}

webapp 이름이 ROOT가 아니며 path가 `/`일 때, 예를 들어, 컨텍스트 설정이 다음과 같으며:

```xml
<!-- 생략 -->
<Host name="localhost" appBase="webapps"
		unpackWARs="true" autoDeploy="true">
	<!-- 생략 -->
	<Context docBase="backweb" path="/" reloadable="true" privileged="true"/>
</Host>
<!-- 생략 -->
```

backweb.war를 webapps 경로에 두고 톰캣을 구동하면

![](/images/tomcat-webapp-location-explorer-1.png)

backweb.war는 ROOT 폴더로 압축이 해제되며 이를 복사한 backweb 폴더가 하나 더 만들어진다.

그리고 톰캣은 backweb 폴더를 무시하고 ROOT를 본다. 진짜임ㅋ 근데 이 상태에서 재시작을 하다보면 어느 순간 ROOT를 안보고 backweb을 본다. 개판이다.

컨텍스트 설정에 따라 작동이 달라지는건 같기도 하다.
