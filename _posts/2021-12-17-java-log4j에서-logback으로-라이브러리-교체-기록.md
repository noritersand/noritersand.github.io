---
layout: post
date: 2021-12-17 13:09:45 +0900
title: '[Java] Log4j에서 Logback으로 라이브러리 교체 기록'
categories:
  - misc
tags:
  - record
  - java
  - spring
  - log4j
  - slf4j
  - logback
---

* Kramdown table of contents
{:toc .toc}

새 회사 오자마자 log4j 이슈 터져서 logback으로 교체 작업했고, 설정 파일 기록 남김.

#### 버전 정보

- Java 14
- Spring 4.3.4
- log4j 1.2.15
- slf4j 1.6.6
- logback 1.2.9

## application

### pom.xml

```xml
  <dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.2.9</version>
  </dependency>
  <dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.9</version>
  </dependency>
  <dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-access</artifactId>
    <version>1.2.9</version>
  </dependency>
  <dependency>
    <groupId>org.logback-extensions</groupId>
    <artifactId>logback-ext-spring</artifactId>
    <version>0.1.5</version>
  </dependency>
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>jcl-over-slf4j</artifactId>
    <version>1.7.5</version>
  </dependency>
  <dependency>
    <groupId>org.bgee.log4jdbc-log4j2</groupId>
    <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
    <version>1.16</version>
  </dependency>
```

### web.xml

```xml
  <listener>
    <listener-class>ch.qos.logback.ext.spring.web.LogbackConfigListener</listener-class>
  </listener>
  <context-param>
    <param-name>logbackConfigLocation</param-name>
    <param-value>classpath:config/logback-${spring.profiles.active:local}.xml</param-value>
  </context-param>
```

### logback-local.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xml>
<configuration scan="true" scanPeriod="1 seconds">
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%-5level|%logger#%method: %msg%n</pattern>
        </encoder>
    </appender>

    <logger name="org.springframework.core" level="info"/>
    <logger name="org.springframework.beans" level="info"/>
    <logger name="org.springframework.context" level="info"/>
    <logger name="org.springframework.web" level="info"/>
    <logger name="org.springframework.security" level="info"/>

    <logger name="jdbc.sqlonly" level="warn"/>
    <logger name="jdbc.connection" level="warn"/>
    <logger name="jdbc.audit" level="warn"/>
    <logger name="jdbc.resultset" level="warn"/>
    <logger name="jdbc.resultsettable" level="info"/>
    <logger name="jdbc.sqltiming" level="debug"/>

    <logger name="ch.qos.logback" level="warn" />

    <root level="DEBUG">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

### log4jdbc.log4j2.properties

```
log4jdbc.drivers=org.mariadb.jdbc.Driver
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0
```

### context-datasource.xml

```xml
  <bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean">
    <property name="jndiName" value="java:comp/env/jdbc/MY_JNDI_NAME" />
  </bean>
```

## WAS

이 프로젝트 요건이 JNDI라서 톰캣 설정에 드라이버 정보 명시:

### server.xml

```xml
  <GlobalNamingResources>
    <Resource auth="Container" driverClassName="net.sf.log4jdbc.sql.jdbcapi.DriverSpy" name="jdbc/MY_JNDI_NAME" password="PSWD" type="javax.sql.DataSource" url="jdbc:log4jdbc:mariadb://IP_ADDRESS_OR_DOMAIN:3306/DATABASE_NAME" username="USER_NAME"/>
  </GlobalNamingResources>
```

### context.xml

```xml
  <ResourceLink name="jdbc/MY_JNDI_NAME" global="jdbc/MY_JNDI_NAME" type="javax.sql.DataSource" />
```
