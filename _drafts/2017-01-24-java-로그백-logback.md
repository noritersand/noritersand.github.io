---
layout: post
date: 2017-01-24 10:10:00 +0900
title: '[Java] 로그백 LOGBack'
categories:
  - java
tags:
  - java
  - logback
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://logback.qos.ch/](https://logback.qos.ch/)
- [https://logback.qos.ch/manual/layouts.html](https://logback.qos.ch/manual/layouts.html)

## 환경 설정

야ㅑㅑㅑ

## 필터링

으아아아

## 뭐시기

호옹이

## 로거 레벨 설정

루트와 일반 로거의 관계.

로거는 루트의 설정(레벨, 어펜더)을 상속받는걸로 추정됨.

## 로컬/안로컬 구분

뭐 이런게 있드라

```xml
  ...
    <springProfile name="local">
      ...
    </springProfile>
    <springProfile name="!local">
      ...
    </springProfile>
  ...
```

## example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xml>
<configuration scan="true" scanPeriod="5 seconds">
    <if condition='property("spring.profiles.active").equals("dev")'>
        <then>
            <property name="LOG_HOME" value="/usr/local/etbslogs" />
        </then>
        <else>
            <property name="LOG_HOME" value="/etbslogs" />
        </else>
    </if>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <filter class="ch.qos.logback.core.filter.EvaluatorFilter">
            <evaluator>
                <expression>
                    return message.contains("loginSql.getAuthorityYnByApproachUrl")
                            || message.contains("userMenuFavoriteSql.getUserMenuFavoriteList");
                </expression>
            </evaluator>
            <OnMatch>DENY</OnMatch>
            <OnMismatch>NEUTRAL</OnMismatch>
        </filter>
        <encoder>
            <!-- <pattern>%-5level: %msg%n</pattern> -->
            <pattern>%-5level: %logger#%method: %msg%n</pattern>
            <!-- <pattern>%-5level: %thread: %logger#%method: %msg%n</pattern> -->
            <!-- <pattern>%date{ISO8601} %-5level: %logger - %msg%n</pattern> -->
            <!-- official example -->
            <!-- <pattern>%date{HH:mm:ss.SSS} [%thread] %-5level: %logger{36} - %msg%n</pattern> -->
        </encoder>
    </appender>
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_HOME}/backweb.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/backweb.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>60</maxHistory> <!-- 60일이 지난 파일은 삭제 -->
        </rollingPolicy>
        <encoder>
            <pattern>%date{ISO8601} %-5level: %logger - %msg%n</pattern>
        </encoder>
    </appender>
    <appender name="ERROR_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <file>${LOG_HOME}/backweb-error.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/backweb-error.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>60</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%date{ISO8601} %-5level: %logger - %msg%n</pattern>
        </encoder>
    </appender>
    <appender name="DATASOURCE_ROUTE_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_HOME}/datasourceroute.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/datasourceroute.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>60</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%date{ISO8601} %-5level: %logger - %msg%n</pattern>
        </encoder>
    </appender>

    <logger name="com.etbs.backweb" level="DEBUG" />
    <logger name="datasourceroute" level="ALL">
        <appender-ref ref="DATASOURCE_ROUTE_FILE" />
    </logger>

    <!-- spring -->
    <logger name="org.springframework" level="DEBUG" />
    <logger name="org.springframework.core" level="WARN" />
    <logger name="org.springframework.web" level="WARN" />
    <logger name="org.springframework.context" level="WARN" />
    <logger name="org.springframework.beans" level="WARN" />
    <logger name="org.springframework.aop" level="WARN" />
    <logger name="org.springframework.transaction" level="WARN" />
    <logger name="org.springframework.jndi" level="WARN" />
    <logger name="org.springframework.ui" level="WARN" />

    <!-- apache -->
    <logger name="org.apache" level="DEBUG" />
    <logger name="org.apache.ibatis.io.ResolverUtil" level="WARN" />

    <!-- 실제 실행되는 쿼리 -->
    <logger name="jdbc.sqltiming" level="DEBUG" />

    <!-- sql -->
    <logger name="org.apache.ibatis" level="DEBUG" />
    <logger name="jdbc" level="WARN" />
    <logger name="com.etbs.backweb.dao" level="WARN" />
    <logger name="org.apache.tomcat.jdbc.pool.PooledConnection.validate" level="WARN" />
    <logger name="org.springframework.jdbc.datasource.DataSourceUtils" level="WARN" />

    <root level="ERROR">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="FILE" />
        <appender-ref ref="ERROR_FILE" />
    </root>
</configuration>
```

로그백은 VM 재시작 없이 바로 적용되는데 이게 톰캣일 경우 reloadable이 true이면 로그백 파일이 수정되도 재시작해버리니 주의할 것
