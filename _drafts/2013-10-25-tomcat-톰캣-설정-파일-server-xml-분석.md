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
        <Context docBase="tomcat-test2" path="/tomcat-test2" reloadable="true" />
      </Host>
    </Engine>
  </Service>
</Server>
```


## Server

최상위 태그


## GlobalNamingResources

`Server > GlobalNamingResources`


## Service

Server > Service


## Connector

`Server > Service > Connector`

#### 속성

- redirectPort
- parseBodyMethods


## Engine

`Server > Service > Engine`


## Realm

`Server > Service > Engine > Realm`


## Host

`Server > Service > Engine > Host`

어플리케이션 단위의 설정을 지정하는 항목. 톰캣이 관리하는 앱 만큼 `Host`가 늘어난다.

#### 속성

TODO 아래 해석하고 지워:

> name: Usually the network name of this virtual host, as registered in your Domain Name Service server. Regardless of the case used to specify the host name, Tomcat will convert it to lower case internally. One of the Hosts nested within an Engine MUST have a name that matches the defaultHost setting for that Engine. See Host Name Aliases for information on how to assign more than one network name to the same virtual host. The name can not contain a wildcard, this is only valid in an Alias.

- name: Request URL의 호스트와 매칭되는 값. 앱을 식별할 호스트 이름으로 사용되며 유일한 이름으로 지정해야한다. 이 값을 `localhost`로 설정한 호스트가 기본 호스트이며, 기본 호스트는 반드시 하나는 있어야 한다. 이 값을 `localhost`가 아닌 값으로 설정한 경우 Request URL의 호스트와 매칭되는데 매칭되는 호스트가 없으면 기본 호스트로 연결한다. 예를 들어 name이 각각 `localhost`와 `abc.com`인 Host가 두 개라면, 아이피 접속일 땐 `localhost`로, abc.com으로 접속하면 `abc.com`로 연결하는 식이다.
- appBase
- unpackWARs
- autoDeploy


## Valve

`Server > Service > Engine > Host > Valve`


## Context

`Server > Service > Engine > Host > Context`

`<Context>`는 둘 이상일 수 있다. 

#### 속성

- docBase: 
- path: context path로 알려져 있는 그 것.
- reloadable: 
- source: 
- sessionCookieName: JSESSIONID를 별도로 지정하는 속성

`sessionCookieName`은 서버 도메인이 겹치지 않는 한 사용할 일이 없다. 그러니까 localhost로 포트번호만 바꿔서 기동하는 로컬 서버가 여러 개면 설정해야 하는 속성이다. 이 설정이 없으면 도메인이 같은 서버를 번갈아 접속할 때마다 JSESSIONID 값이 변경되면서 세션 유지가 안되는 현상이 발생한다.

`sessionCookieName` 설정은 아래처럼:

```xml
        <Context path="/" sessionCookieName="BOO-JSESSIONID" />
```

`path`는 생략하면 제대로 작동하지 않으니 필수, 나머지는 생략해도 된다. 이렇게 하면 `JSESSIONID` 대신 `BOO-JSESSIONID` 쿠키가 생성되며, 해당 서버는 `BOO-JESSIONID`의 값을 세션 키로 사용하게 된다.

그리고 개발자도구로 확인해보면 다른 쿠키가 보일 수도 있는데 도메인이 같아서 보이는 것 뿐이니 무시할 것.


## Alias

`Server > Service > Engine > Host > Alias`

`<Host>`의 별칭을 지정한다. `name`을 여러 개 두는 효과가 있(는 걸로 추정된)다.

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
