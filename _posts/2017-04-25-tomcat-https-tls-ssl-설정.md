---
layout: post
date: 2017-04-25 11:40:00 +0900
title: '[Tomcat] HTTPS(TLS/SSL) 설정'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - https
  - tls
  - ssl
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html](https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html)
- [https://tomcat.apache.org/tomcat-8.5-doc/config/http.html](https://tomcat.apache.org/tomcat-8.5-doc/config/http.html)
- [https://www.lesstif.com/pages/viewpage.action?pageId=17105864](https://www.lesstif.com/pages/viewpage.action?pageId=17105864)
- [http://devhome.tistory.com/64](http://devhome.tistory.com/64)
- [http://visu4l.tistory.com/419](http://visu4l.tistory.com/419)


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

## 버그

HTTPS 커넥터의 포트를 8443으로, HTTP 커넥터의 포트를 8080으로 했을 때, 먼저 8443으로 접속한 뒤 8080으로 접속하면 JSESSIONID가 쿠키 목록에 보이지 않는다. 이 현상은 크롬에서만 발견되는데, secure 쿠키와 non-secure 쿠키가 동시에 존재할 때 (예를 들면 HTTPS를 최초로 접속하고 HTTP로 이동했으며 두 페이지의 호스트명이 일치할 때 secure 쿠키와 non-secure 쿠키가 동시에 존재한다) set-cookie 헤더를 크롬이 무시해서 나타나는 현상이다.

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
