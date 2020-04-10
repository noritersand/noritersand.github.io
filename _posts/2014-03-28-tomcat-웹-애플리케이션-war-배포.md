---
layout: post
date: 2014-03-28 20:42:00 +0900
title: '[Tomcat] 웹 애플리케이션(WAR) 배포'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - deploy
  - distribute
  - war
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://tomcat.apache.org/tomcat-8.0-doc/config/context.html](https://tomcat.apache.org/tomcat-8.0-doc/config/context.html)
- [https://tomcat.apache.org/tomcat-9.0-doc/config/context.html](https://tomcat.apache.org/tomcat-9.0-doc/config/context.html)
- [http://blog.daum.net/naline1213/7592254](http://blog.daum.net/naline1213/7592254)

#### 테스트 환경

- JDK 1.7.0_51
- tomcat 7.0.52
- eclipse kepler

## export WAR

먼저 배포할 프로젝트를 WAR로 추출한다. 참고로 WAR는 web application archive의 약자로 웹 애플리케이션을 배포하기 위한 파일들의 압축이다. [이게](https://namu.wiki/w/WAAAGH!!) 아니다.

추출에 사용된 툴과 옵션에 따라 내용은 다를 수 있다. 가령 이클립스에서 생성한 Dynamic Web Project를 Export 할 때 서버 런타임을 톰캣으로 선택한다면 해당 프로젝트-WebContent 하위의 모든 폴더와 파일을 내보내게 된다. 프로젝트에 자바 소스가 존재하면 컴파일된 클래스 파일을 WAR에 포함시키며 WEB-INF/classes 아래 경로에 위치하게 된다.

팁: JDK의 jar.exe로도 WAR를 만들 수 있다.

![▲ 이클립스의 export WAR](/images/eclipse-webapp-extract-to-war.png)
▲ 이클립스의 export WAR

## configuration & deploy

WAR를 톰캣으로 배포하는 방법은 두 가지다:

- 톰캣에서 제공하는 GUI 툴인 `/manager/html` 페이지에서 설정
- `톰캣경로\conf\server.xml` 파일을 통한 webapp 설정

## 톰캣 매니저를 통한 배포

톰캣 매니저 접속 권한을 설정한다. tomcat-users.xml을 열어 아래처럼 변경한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">

    <role rolename="manager-script"/>
    <role rolename="manager-gui"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <user username="admin" password="1234" roles="manager-gui,manager-script,manager-status,manager-jmx"/>

</tomcat-users>
```

톰캣경로/bin/startup.bat 을 실행해 서버를 시작한다.

server.xml을 수정하지 않았다면 서버 리스너의 기본 포트는 8080이다. 브라우저에서 `localhost:8080/manager/html` 을 입력해 직접 매니저 페이지로 이동하거나 `localhost:8080` 만 입력해 나오는 인덱스 페이지에서 `Manager App` 버튼을 클릭한다.

톰캣 매니저 페이지 하단의 `Deploy` - `WAR file to deploy` 에서 WAR파일을 선택해 'Deploy'한다.

페이지 상단의 배포중인 애플리케이션을 관리하는 `Applications`에서 배포한 애플리케이션을 활성화 한다.

배포가 완료되었으며 `localhost:8080/WAR파일명`으로 접근할 수 있다.

## server.xml 설정을 통한 배포

WAR 파일을 `톰캣경로\webapps` 아래에 둔다. 이때 압축은 풀어도 되고 풀지 않아도 된다.

![](/images/webapp-extract-to-war-via-server-xml.png)

`톰캣경로\conf\server.xml` 을 다음처럼 수정한다:

```xml
<Host name="localhost" appBase="webapps"
            unpackWARs="true" autoDeploy="true">
    <!-- 생략 -->
    <Context docBase="logictest" path="/" reloadable="true"/>
</Host>
```

사실 이 단계를 거치지 않아도 1번 이후 바로 접근할 수 있긴 하다. 다만 이를 통해 ContextPath를 설정할 수 있다는 것을 알아두자. (ContextPath: 서버이름(hostname) 바로 다음에 오는 경로. 여기선 path를 `/`로 설정했으므로 ContextPath는 없다고 보면 된다.)

서버를 시작하면 배포가 완료된다. 2번에서 server.xml을 수정했으므로 `localhost:8080`으로 접근할 수 있다.

참고로 Context 설정은 server.xml을 통해 직접 설정하는 것보다 `docBase/META-INF/context.xml`을 추가해 별도로 관리하는것이 권장된다. (docBase: appBase 아래에 위치한 폴더나 WAR를 의미)
