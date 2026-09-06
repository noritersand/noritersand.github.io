---
layout: post
date: 2017-03-17 00:00:00 +0900
title: '[Tomcat] Tomcat 노트'
categories:
  - tomcat
tags:
  - infrastructure
  - was
  - tomcat
  - web-application-server
---

* Kramdown table of contents
{:toc .toc}


## 개요

톰캣 관련 글 모음


## 톰캣 JNDI 작성 예시

#### 참고 문서

- <http://tomcat.apache.org/tomcat-8.0-doc/jndi-resources-howto.html>
- <http://tomcat.apache.org/tomcat-9.0-doc/jndi-resources-howto.html>
- [http://beyondj2ee.tumblr.com/post/14508592466/tomcat-7-환경에서-jndi-datasource-spring-연동-방법](http://beyondj2ee.tumblr.com/post/14508592466/tomcat-7-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-jndi-datasource-spring-%EC%97%B0%EB%8F%99-%EB%B0%A9%EB%B2%95)

### context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- ... ... <Valve className="org...... -->

<Resource name="jdbc/myoracle" type="javax.sql.DataSource"
    driverClassName="oracle.jdbc.driver.OracleDriver"
    url="jdbc:oracle:thin:@127.0.0.1:1521:orcl"
    username="noritersand" password="java1234$!" maxActive="20" maxIdle="10"
    maxWait="-1" />
</Context>
<!-- 이후 connection 정보가 바뀔때는 이 파일만 수정한다. -->
```

### web.xml

```xml
<!-- 생략 -->

<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>

<!-- DBCP -->
<resource-ref>
    <res-ref-name>jdbc/myoracle</res-ref-name>
    <res-type>javax.sql.DataSource</res-type>
</resource-ref>

<!-- 생략 -->
```

### DBCPConn.java

```java
import java.sql.Connection;
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

public class DBCPConn {
    private static Connection conn;

    private DBCPConn() {
    }

    public static Connection getConnection() {
        if (conn == null) {
            try {
                Context init = new InitialContext();
                // java:/comp/env -> 이름으로 바인딩된 객체를 검색
                Context content = (Context) init.lookup("java:/comp/env");
                DataSource ds = (DataSource) content.lookup("jdbc/myoracle");
                conn = ds.getConnection();
            } catch (Exception e) {
                System.out.println(e);
            }
        }
        return conn;
    }

    public static void close() {
        if (conn != null) {
            try{
                if (!conn.isClosed()) {
                    conn.close();
                }
            } catch (Exception e) {
                System.out.println(e);
            }
        }
        conn = null;
    }
}
```

### 객체 생성 확인

```jsp
<%= DBCPConn.getConnection() %>
```

![](/images/jndi-tomcat-1.png)


## 웹 애플리케이션(WAR) 배포

#### 참고 문서

- <https://tomcat.apache.org/tomcat-8.0-doc/config/context.html>
- <https://tomcat.apache.org/tomcat-9.0-doc/config/context.html>
- <http://blog.daum.net/naline1213/7592254>

#### 테스트 환경

- JDK 1.7.0_51
- tomcat 7.0.52
- eclipse kepler

### export WAR

먼저 배포할 프로젝트를 WAR로 추출한다. 참고로 WAR는 web application archive의 약자로 웹 애플리케이션을 배포하기 위한 파일들의 압축이다. [이게](https://namu.wiki/w/WAAAGH!!) 아니다.

추출에 사용된 툴과 옵션에 따라 내용은 다를 수 있다. 예를 들어 이클립스에서 생성한 Dynamic Web Project를 Export 할 때 서버 런타임을 톰캣으로 선택한다면, `프로젝트/WebContent` 디렉터리 하위의 모든 디렉터리와 파일을 내보내게 된다. 프로젝트에 Java 소스가 존재하면 컴파일된 클래스 파일을 WAR에 포함시키며 `WEB-INF/classes` 아래 경로에 위치하게 된다.

팁: JDK의 `jar.exe`로도 WAR를 만들 수 있다.

![▲ 이클립스의 export WAR](/images/eclipse-webapp-extract-to-war.png)
▲ 이클립스의 export WAR


### configuration & deploy

WAR를 톰캣으로 배포하는 방법은 두 가지다:

- 톰캣에서 제공하는 GUI 툴인 `/manager/html` 페이지에서 설정
- `톰캣경로\conf\server.xml` 파일을 통한 webapp 설정

### 톰캣 매니저를 통한 배포

톰캣 매니저 접속 권한을 설정한다. `tomcat-users.xml`을 열어 아래처럼 변경한다.

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

`톰캣경로/bin/startup.bat` 을 실행해 서버를 시작한다.

server.xml을 수정하지 않았다면 서버 리스너의 기본 포트는 8080이다. 브라우저에서 `localhost:8080/manager/html` 을 입력해 직접 매니저 페이지로 이동하거나 `localhost:8080` 만 입력해 나오는 인덱스 페이지에서 `Manager App` 버튼을 클릭한다.

톰캣 매니저 페이지 하단의 `Deploy` - `WAR file to deploy` 에서 WAR파일을 선택해 'Deploy'한다.

페이지 상단의 배포중인 애플리케이션을 관리하는 `Applications`에서 배포한 애플리케이션을 활성화 한다.

배포가 완료되었으며 `localhost:8080/WAR파일명`으로 접근할 수 있다.

### server.xml 설정을 통한 배포

WAR 파일을 `톰캣경로\webapps` 아래에 둔다. 이때 압축은 풀어도 되고 풀지 않아도 된다.

![](/images/webapp-extract-to-war-via-server-xml.png)

`톰캣경로\conf\server.xml` 을 다음처럼 수정한다:

```xml
<Host name="localhost" appBase="webapps"
            unpackWARs="true" autoDeploy="false">
    <!-- 생략 -->
    <Context docBase="logictest" path="/" reloadable="true"/>
</Host>
```

사실 이 단계를 거치지 않아도 1번 이후 바로 접근할 수 있긴 하다. 다만 이를 통해 ContextPath를 설정할 수 있다는 것을 알아두자. (`ContextPath`: 서버이름(`hostname`) 바로 다음에 오는 경로. 여기선 `path`를 `/`로 설정했으므로 `ContextPath`는 없다고 보면 된다)

서버를 시작하면 배포가 완료된다. 2번에서 `server.xml`을 수정했으므로 `localhost:8080`으로 접근할 수 있다.

참고로 Context 설정은 `server.xml`을 통해 직접 설정하는 것보다 `docBase/META-INF/context.xml`을 추가해 별도로 관리하는것이 권장된다. (`docBase`: `appBase` 아래에 위치한 디렉터리나 WAR를 의미)


## 아파치-톰캣 연동

#### 참고 문서

- <http://tomcat.apache.org/connectors-doc/webserver_howto/apache.html>
- <http://www.easywayserver.com/apache-tomcat-integration.html>
- <http://wiki.gurubee.net/pages/viewpage.action?pageId=1507883>
- <http://wiki.gurubee.net/pages/viewpage.action?pageId=26739703>
- <http://www.lesstif.com/pages/viewpage.action?pageId=12943367>
- <http://joont.tistory.com/55>
- <http://httpd.apache.org/docs/2.4/ko/vhosts/examples.html>
- <http://httpd.apache.org/docs/2.4/ko/vhosts/mass.html>

#### 테스트 환경

- apache 2.4.20
- tomcat 8.x

### 전제 조건

- 아파치와 톰캣은 설치되어 있다 가정.
- 두 개의 호스트를 각각의 WAS로 구동(하나의 아파치에 톰캣 둘)

**TODO** mod_jk.so 설치방법 추가

**TODO** VirtualHost 설정방법 추가

### Apache Tomcat Connectors

우선 JK 모듈이 필요한데 JK 모듈은 The Apache Tomcat Connectors를 지칭하는 말로 아파치-톰캣 연동에 필요한 모듈이며 [AJP](https://en.wikipedia.org/wiki/Apache_JServ_Protocol)로 통신한다.

mod_jk.so를 `아파치설치경로/modules` 아래에 위치시킨다. mod_jk.so는 OS에 따라 적합한 파일이 다르다. 리눅스 환경이라면 컴파일을 직접 해야 할 수도 있다. 만약 아파치 구동 중 'invalid ELF header' 에러가 발생하면 mod_jk.so가 뭔가 잘못된것이다. (컴파일 버전이 다르다던지)

JK 모듈 설치를 완료했으면 `/httpd/conf/httpd.conf`에 다음을 추가한다:

#### httpd.conf

```bash
Listen 80

Include conf.modules.d/*.conf

# 아래를 추가
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info
JkMount /bo/* worker1
JkMount /* worker2
```

JkMount는 요청을 처리할 WAS를 의미하는데 위의 경우 요청된 주소의 PATH가 `/bo/`로 시작하면 worker1에, 그 외 모든 주소는 worker2에 포워딩 된다. JkMount 설정이 여러 개 있을때는 상단에 있는 것이 우선순위가 높다.

현재까진 JkMount로 명시한 worker1과 worker2를 찾을 수 없다. 따라서 `/httpd/conf/workers.properties`를 다음처럼 작성한다:

#### workers.properties

```bash
# HTTPD Web Server and Apache Tomcat (ajp) Connector
# the loadbalancer configuration of the Server
#
# Include workers.properties by conf/extra/httpd-modjk.conf
#
# Define loadbalancer 2 worker node using ajp13
# admin Cluster Group 1 #############################################
#
# configuration template
worker.template.type=ajp13
#worker.template.lbfactor=1
#worker.template.socket_timeout=30
#worker.template.socket_keepalive=true
#worker.template.recovery_options=7
#worker.template.ping_mode=A
#worker.template.ping_timeout=10000
#worker.template.connection_pool_size=25
#worker.template.connection_pool_minsize=25
#worker.template.connection_pool_timeout=60

worker.list=worker1, worker2

worker.worker1.reference=worker.template
worker.worker1.port=8009
worker.worker1.host=localhost

worker.worker2.reference=worker.template
worker.worker2.port=8010
worker.worker2.host=localhost

#worker.wlb.type=lb
#worker.wlb.retries=2
#worker.wlb.method=Session
#worker.wlb.sticky_session=True
#worker.wlb.balance_workers=worker1, worker2
```

각 톰캣의 `톰캣설치경로/conf/server.xml`의 설정을 다음처럼 수정한다:

#### server.xml

`bo(worker1)`:

```bash
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443"/>
<!-- 생략 -->
<Context docBase="back-web" path="/bo" reloadable="false"/>
```

`fo(worker2)`:

```bash
<Connector port="8010" protocol="AJP/1.3" redirectPort="8443"/>
<!-- 생략 -->
<Context docBase="front-web" path="/" reloadable="false"/>
```

여기서 docBase는 톰캣의 webapps 아래에 위치한 디렉터리 혹은 WAR파일을 말한다.

![](/images/apache-tomcat-connect-1.png)

### 서버 기동

#### 아파치

```bash
httpd -k start
```

이 때 필요한 경우 서비스(리눅스는 데몬) 이름을 지정할 수 있다:

```bash
httpd -k install -n second-apache
httpd -k start -n second-apache
```

이름 지정은 여러 인스턴스를 실행해야 할 때 사용함.

#### 톰캣

```bash
startup.bat  # 혹은 startup.sh
```

에러 없이 성공하면 아래 주소로 테스트:

- http://localhost/bo/
- http://localhost

### VirtualHost

하나의 아파치에서 도메인에 따라 서비스를 분리할 때 사용하는 모듈. 일단 아래처럼만 해도 돌아감. 자세한건 나중에 정리

```bash
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info

<virtualhost *:80>
    ServerName bo.test.com
    DocumentRoot /home/user1/was/bo/webapps/back-web/
    JkMount /* worker1
</virtualhost>

<virtualhost *:80>
    ServerName fo.test.com
    DocumentRoot /home/user1/was/fo/webapps/front-web/
    JkMount /* worker2
</virtualhost>
```

hosts 설정하고 아래 주소로 테스트:

- http://bo.test.com
- http://fo.test.com

```apache
Configuring Apache to serve static web application files

If the Tomcat Host appBase (webapps) directory is accessible by the Apache web server, Apache can be configured to serve web application context directory static files instead of passing the request to Tomcat.


Caution: For security reasons it is strongly recommended that JkMount is used to pass all requests to Tomcat by default and JkUnMount is used to explicitly exclude static content to be served by httpd. It should also be noted that content served by httpd will bypass any security constraints defined in the application's web.xml.

Use Apache's Alias directive to map a single web application context directory into Apache's document space for a VirtualHost:

# Static files in the examples webapp are served by apache
Alias /examples /vat/tomcat3/webapps/examples

# All requests go to worker1 by default
JkMount /* worker1

# Serve html, jpg and gif using httpd
JkUnMount /*.html worker1
JkUnMount /*.jpg  worker1
JkUnMount /*.gif  worker1
```


## webapp.root

```bash
정보: Starting Servlet Engine: Apache Tomcat/8.5.8
6월 08, 2017 7:42:47 오후 org.apache.jasper.servlet.TldScanner scanJars
정보: At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
6월 08, 2017 7:42:47 오후 org.apache.catalina.core.ApplicationContext log
정보: No Spring WebApplicationInitializer types detected on classpath
6월 08, 2017 7:42:47 오후 org.apache.catalina.core.ApplicationContext log
정보: Set web app root system property: 'webapp.root' = [C:\project\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp1\wtpwebapps\frontweb\]
DEBUG: Initializing filter 'encodingFilter'
DEBUG: Filter 'encodingFilter' configured successfully
```

```java
${webapp.root}
```

이게 그냥 설정되진 않고 뭔가를 해야 되는거 같은데...
로깅 프레임워크에서 설정한다는 말도 있고.

web.xml을 쓰지 않고 @WebListener(혹은 ServletContextListener의 구현체)를 사용할땐 안 되는것 같다.

톰캣 플러그인에서 띄운건 저 로그가 뜨는데, 톰캣으로 직접 띄우면 없다.

이걸 해야 하는것 같기도 하고 말이징

```xml
<context-param>
    <param-name>webAppRootKey</param-name>
    <param-value>ecbase.root</param-value>
</context-param>
```

어차피 저 값은 웹루트 경로에 해당하고 이 경로를 구하는 값이 저것만 있지 않으니 몰라도 되지 않을까? 싶어도 로그백 설정 파일에서 필요할 수 있다.

```java
String root = req.getSession().getServletContext().getRealPath("/"); // 이 값과 webapp.root는 같음
```

쏘 미스테리


## webapp 이름이 ROOT가 아니며 path가 "/"일 때

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

backweb.war는 ROOT 디렉터리로 압축이 해제되며 이를 복사한 backweb 디렉터리가 하나 더 만들어진다.

그리고 톰캣은 backweb 디렉터리를 무시하고 ROOT를 본다. 진짜임ㅋ 근데 이 상태에서 재시작을 하다보면 어느 순간 ROOT를 안보고 backweb을 본다.

컨텍스트 설정에 따라 작동이 달라지는건 같기도 하다.


## HTTPS (TLS/SSL) 설정

#### 참고 문서

- <https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html>
- <https://tomcat.apache.org/tomcat-8.5-doc/config/http.html>
- <https://www.lesstif.com/pages/viewpage.action?pageId=17105864>
- <http://devhome.tistory.com/64>
- <http://visu4l.tistory.com/419>

### 방ㅂ법

콘솔에서 키 생성 (JDK가 설치되어 있어야 함):

```bash
keytool -genkey -alias tomcat -keyalg RSA
```

server.xml에 다음 항목 추가:

```xml
<Connector SSLEnabled="true" clientAuth="false"
        keystorefile="${userprofile}\.keystore" keystorePass="123456"
        maxThreads="150" port="8443" scheme="https" secure="true" sslProtocol="TLS" />
```

https://localhost:8443 접속 테스트

끗.

### 버그

HTTPS 커넥터의 포트를 8443으로, HTTP 커넥터의 포트를 8080으로 했을 때, 먼저 8443으로 접속한 뒤 8080으로 접속하면 JSESSIONID가 쿠키 목록에 보이지 않는다. 이 현상은 초로미에서만 발견되는데, secure 쿠키와 non-secure 쿠키가 동시에 존재할 때 (예를 들면 HTTPS를 최초로 접속하고 HTTP로 이동했으며 두 페이지의 호스트명이 일치할 때 secure 쿠키와 non-secure 쿠키가 동시에 존재한다) set-cookie 헤더를 Chrome이 무시해서 나타나는 현상이다.

한 번 이렇게 되어버리면 secure 쿠키가 우선권을 갖게 되며 secure 쿠키를 날리기 전까지 non-secure 쿠키는 생성되지 않는다.

누군가 아래같은 해결방법을 제시했지만... 안된다.

```xml
<session-config>
    <cookie-config>
        <name>JSESSIONID</name>
        <secure>false</secure>
    </cookie-config>
</session-config>
```

2017년 기준, 아직까진 HTTP 페이지를 강제로 먼저 접속하게 하는 방법 외에는 대책이 음슴.


## 톰캣 메모리 노트

#### 참고 문서

- [덤프 떠서 메모리 leak 확인](http://atin.tistory.com/440)
- <http://blog.naver.com/salsu0/30000025219>
- <http://javaslave.tistory.com/23>
- <http://d2.naver.com/helloworld/37111>
- <https://www.cubrid.org/blog/how-to-tune-java-garbage-collection>

```bash
export JAVA_OPTS="$JAVA_OPTS -Djava.awt.headless=true -server -Xms1024m -XX:NewSize=256m"
```

여기서 `java.awt.headless=true`는 리눅스의 이미지 리사이징 관련 옵션이다.

아래는 주로 사용하는 옵션:

```bash
# 인코딩
export JAVA_OPTS="$JAVA_OPTS -Dfile.encoding=UTF-8"

# 웹앱에서 사용하는 argument들
export JAVA_OPTS="$JAVA_OPTS -Dtomcat.connector.http.port=7001"
export JAVA_OPTS="$JAVA_OPTS -Djava.awt.headless=true"
export JAVA_OPTS="$JAVA_OPTS -Dspring.profiles.active=real"
export JAVA_OPTS="$JAVA_OPTS -Dweb.app.verifymode=usingcert"
export JAVA_OPTS="$JAVA_OPTS -Dlog4jdbc.dump.sql.maxlinelength=0"
export JAVA_OPTS="$JAVA_OPTS -Dapp.log.home=/usr/local/tomcatlogs/app6/1"

# 메모리 설정
export JAVA_OPTS="$JAVA_OPTS -server -Xmx1024m -Xms1024m -XX:MaxNewSize=384m"

# 기타
export JAVA_OPTS="$JAVA_OPTS -Xverify:none"
```

아래는 메모리 관련 에러났을 때 덤프파일 만드는 옵션:

```bash
JAVA_OPTS="-Djava.awt.headless=true -server -Xmx1024m -Xms1024m -XX:MaxNewSize=384m -XX:-HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/usr/local/tomcat8.5/bin -XX:ParallelGCThreads=2 -XX:-UseConcMarkSweepGC -XX:-PrintGC -XX:-PrintGCDetails -XX:-PrintGCTimeStamps -XX:-TraceClassUnloading -XX:-TraceClassLoading"
```

### 누군가의 말

> 32bit JVM에 보통 메모리 할당한계가 4G에요
이전 32bit Windows가 4G이상 인식못했던것처럼
그중 반은 OS가 반은 VM App이 쓸수있으니 2G
저렇게 1기가로 최대 최소 메모리는 잡는것은
PermGen이라는 힙이외의 class정보등이 올라가는 영역을합치면
대충 프로세스가 1.5G정도되죠.. 그러니까..대략 최대치?
64bit에는 더 써도 되는데
써봤자 GC시간도 많이 걸리고.. 해서 최대 2G는 안넘도록 설정해요
java8부터 Perm이 없어지고 MetaSpace가 대체하는데 이건 무한대인듯.. 기본이


### 구 버전 이클립스에서 메모리 최적화

인디고 정도 쯤 될 때... (perm size와 new size는 512m이 나은걸지도?)

```bash
-XX:PermSize=256m
-XX:MaxPermSize=256m
-XX:NewSize=256m
-XX:MaxNewSize=256m
-Xms1024m
-Xmx1024m
```


## tomcat 8.x 이상 버전에서 톰캣이 생성하는 파일의 권한이 640으로 생성될 때

톰캣으로 구동하는 웹 어플에서 파일을 업로드 할 때 other 사용자한테 읽을 수 있도록 권한이 부여된 파일이 생성되어야 하는데 이상하게도 디렉터리는 750, 파일은 640으로 생성된다.

`bin/catalina.sh`를 보면 umask를 027로 설정하는것이 원인이었고 이 부분을 코멘트 처리하고 재시작하니 644로 생성하더라.


## 톰캣에서 웹앱이 두 번 시작되는 현상

#### 참고 문서

- <https://dezang.github.io/how-to-solve-spring-application-initialized-twice/>


톰캣 server.xml의 컨텍스트 설정이 다음과 같고:

```xml
<!-- 생략 -->
<Host name="localhost" appBase="webapps"
      unpackWARs="false" autoDeploy="false">

  <!-- SingleSignOn valve, share authentication between web applications
       Documentation at: /docs/config/valve.html -->
  <!--
  <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
  -->

  <!-- Access log processes all example.
       Documentation at: /docs/config/valve.html
       Note: The pattern used is equivalent to using pattern="common" -->
  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
         prefix="localhost_access_log" suffix=".txt"
         pattern="%h %l %u %t &quot;%r&quot; %s %b" />

  <Context docBase="mywebroot" path="/" reloadable="false" />
</Host>
<!-- 생략 -->
```

웹앱 web.xml의 서블릿 설정이 다음과 같을 때:

```xml
<!-- 생략 -->
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:/config/dispatcher-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<!-- 생략 -->
```

**서블릿이 두 번 기동되는 현상이 있다.**

`<load-on-startup>1</load-on-startup>`를 삭제하거나 docBase를 ROOT로 변경하면 이 현상이 사라진다.

이게 스프링일 때 그러는건지 web.xml 사용하면 무조건 그러는건지는 아직 몲.

주의: `load-on-startup` 설정이 없으면 톰캣 기동 시에는 웹앱이 시작(스프링일 경우 Initializing Spring FrameworkServlet)되지 않는다. HTTP 요청이 한 번이라도 발생하면 그 때서야 시작한다. 예를 들어 웹앱에 스프링 스케줄러 컴포넌트가 있다면, 톰캣 재기동 후 요청이 한 번이라도 있어야 스케줄러가 작동하는 식.


## server.xml의 autoDeploy 사용시 주의할 점

autoDeploy가 true이고 war가 교체되었을 때, 그리고 docBase 디렉터리 내에 심볼릭 링크가 존재할 때 톰캣이 심볼릭 링크 내의 파일까지도 재귀 삭제해버리는 현상이 발생한다.

심볼릭 링크의 실제 경로를 일회성 혹은 임시 디렉터리로 사용한다면 문제가 없겠지만, 파일을 모아놓고 지속적으로 사용하는 경로라면 낭패를 볼 수 있다.

따라서 docBase내에 심볼릭 링크가 있을 경우 autoDeploy를 false로 설정하던가, 아니면 심볼릭 링크를 appBase 부터 아예 다른 경로로 이동하고 docBase에서 해당 경로를 참조하게 해야 한다.


## Java 아규먼트에 접근하기

톰캣 실행 인자가

```bash
java ... -DhttpPort=8080 ... org.apache.catalina.startup.Bootstrap start
```

일 때

```xml
  <Connector port="${httpPort}" protocol="HTTP/1.1"
    connectionTimeout="20000"
    redirectPort="8443" />
```

이렇게 쓸 수 있음.
</content>
</invoke>
