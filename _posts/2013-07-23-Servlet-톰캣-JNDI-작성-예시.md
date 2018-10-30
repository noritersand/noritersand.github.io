---
layout: post
date: 2013-07-23 02:42:00 +0900
title: 'Servlet: 톰캣 JNDI 작성 예시'
categories:
  - java
tags:
  - java
  - tomcat
  - connection
  - ojbc
  - jndi
  - 코드모음
---

#### context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- ... ... <Valve className="org...... -->

<Resource name="jdbc/myoracle" type="javax.sql.DataSource"
    driverClassName="oracle.jdbc.driver.OracleDriver"
    url="jdbc:oracle:thin:@220.76.176.66:1521:orcl"
    username="noritersand" password="java301$!" maxActive="20" maxIdle="10"
    maxWait="-1" />
</Context>
<!-- 이후 connection 정보가 바뀔때는 이 파일만 수정한다. -->
```

#### web.xml

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

#### DBCPConn.java

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
        if (conn==null) {
            try {
                Context init = new InitialContext();
            //java:/comp/env -> 이름으로 바인딩된 객체를 검색
                Context content = (Context)init.lookup("java:/comp/env");
                DataSource ds = (DataSource)content.lookup("jdbc/myoracle");
                conn = ds.getConnection();
            } catch (Exception e) {
                System.out.println(e);
            }
        }
        return conn;
    }

    public static void close() {
        if (conn!=null) {
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

#### 객체 생성 확인

```xml
<%= DBCPConn.getConnection() %>
```

![](/images/jndi-tomcat-1.png)

끗
