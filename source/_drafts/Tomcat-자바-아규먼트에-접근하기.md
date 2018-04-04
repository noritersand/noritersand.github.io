---
title: 'Tomcat: 자바 아규먼트에 접근하기'
categories:
  - etc
tags:
  - tomcat
  - java
  - todo
---

톰캣 실행 인자가
```bash
java ... -DhttpPort=8080 ... org.apache.catalina.startup.Bootstrap start
```
일 때

```markup
  <Connector port="${httpPort}" protocol="HTTP/1.1"
    connectionTimeout="20000"
    redirectPort="8443" />
```
이렇게 쓸 수 있음.

