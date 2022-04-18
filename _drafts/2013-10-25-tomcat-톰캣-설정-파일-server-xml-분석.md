---
layout: post
date: 2013-10-25 13:42:00 +0900
title: '[Tomcat] 톰캣 설정 파일 server.xml 분석'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - location
  - config
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Apache Tomcat 10 Configuration Reference](https://tomcat.apache.org/tomcat-10.0-doc/config/index.html)
- [Apache Tomcat 9 Configuration Reference](https://tomcat.apache.org/tomcat-9.0-doc/config/index.html)
- [Apache Tomcat 8 Configuration Reference](https://tomcat.apache.org/tomcat-8.5-doc/config/index.html)

## 개요

오쪼구조쪼구

## 작성 예시

8.5.73의 기본 설정(에서 `context`만 추가):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="18080" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>

  <Service name="Catalina">

    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Engine name="Catalina" defaultHost="localhost">

      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

        <Context docBase="tomcat-test" path="/tomcat-test" reloadable="true" />
      </Host>
    </Engine>
  </Service>
</Server>
```

## Server

최상위 태그

TODO

## GlobalNamingResources

구조: `Server > GlobalNamingResources`

TODO

## Service

Server > Service

TODO

## Connector

구조: `Server > Service > Connector`

TODO

## Engine

구조: `Server > Service > Engine`

TODO

## Realm

구조: `Server > Service > Engine > Realm`

TODO

## Host

구조: `Server > Service > Engine > Host`

TODO

#### name

앱을 식별할 호스트 이름으로 사용되며 유일한 이름으로 지정해야한다.

도메인이나 아이피로 설정한 경우 어쩌구저쩌구(**TODO 용어를 잘 모르겠음**) 동일한 호스트로 연결된다.

#### appBase

TODO

#### unpackWARs

TODO

#### autoDeploy

TODO

## Valve

구조: `Server > Service > Engine > Host > Valve`

## Context

구조: `Server > Service > Engine > Host > Context`

#### docBase

#### reloadable

#### path


TODO

---

아래는 이전 작성 내용


설정을 아무것도 변경한게 없으면 톰캣 기동 후 `localhost:8080/` 으로 서버상태를 확인할 수 있다.

`localhost:8080/` 뒤에 추가 경로명이 올 경우 톰캣은 webapps 폴더 아래에서 찾는데 이 설정은 server.xml의 appBase 속성값 변경으로 바꿀 수 있다.

```xml
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="false"
    xmlValidation="false" xmlNamespaceAware="false">

  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
      pattern="%h %l %u %t &quot;%r&quot; %s %b" prefix="localhost_access_log" suffix=".txt"/>

</Host>
```

webapps 폴더 아래에는 ROOT 폴더가 존재하는데 톰캣이 webapps 폴더에서 요청 URL 경로를 찾지 못했을 때는 ROOT 폴더에서 다시 찾기 시작한다. 즉, webapps 아래에 있거나 `webapps/ROOT` 아래에 있거나 접속 URL은 같다.

server.xml 샘플 파일에는 없는 항목이지만 `<Host>` 아래에 `<Context>`를 추가해서 파일경로와 접속 URL을 추가적으로 관리 할 수 있다.

```xml
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="false"
    xmlValidation="false" xmlNamespaceAware="false">

  <!-- 생략 -->

  <Context docBase="test/WebContent" path="/test" reloadable="true"/>
</Host>
```

### reloadable

`WEB-INF/classes` 혹은 `WEB-INF/lib` 폴더의 내용이 변경되었을 때 자동으로 reload 할 것인지 여부.

### path

만약 톰캣을 이클립스에서 관리한다면 이클립스는 자동으로 프로젝트명을 path에 추가한다. 예를 들어 test라는 프로젝트를 이클립스에서 톰캣서버에 추가하면 path는 `/test`이며 접속 URL은 `localhost:port/test`가 된다. 접속 URL의 test명을 생략하거나 변경하고 싶다면 path 속성값을 `/` 혹은 원하는 값으로 설정한다. 참고로 Servlet에서 `Request.ContextPath()` 메서드가 이 값을 반환한다.

### docBase

appBase의 하위 경로이며, 톰캣이 기동하는 각각의 앱을 의미한다.

path에 해당하는 URL이 요청되었을 때 해당 파일을 어느 경로에서 찾을지를 설정한다. appBase로 설정된 경로 아래의 폴더에서 찾기 시작하는데 위의 경우엔 `webapps/test/WebContent` 아래에서 찾는다.

가령 `webapps\test\WebContent\index.jsp`가 있을 때 요청 URL이 `localhost:port/test/index.jsp`면 `appBase\docbase` 아래에 있는 index.jsp를 찾는다.

server.xml의 설정이 기본값일 때 이클립스(메이븐같은 빌드 툴을 이용한 레이아웃이 아니라 이클립스 자체 레이아웃일 때)에서 서버를 구동하는 경우와 톰캣을 직접 구동하는 경우의 차이는 다음과 같다.

- 요청된 URL이 `localhost:8080/test/index.jsp` 일 때
- 이클립스에서 찾는 경로는 `test/WebContent/index.jsp`
- 톰캣에서 찾는 경로 `webapps/test/index.jsp`

가령 요청 URL이 `localhost:8080/test/index.jsp`일 때 이클립스는 `test/WebContent` 아래에서 index.jsp를 찾고 톰캣은 `webapps/test` 아래에서 index.jsp를 찾는다.
