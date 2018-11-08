---
layout: post
date: 2013-07-23 00:05:00 +0900
title: 'Java: 싱글턴패턴으로 connection 객체 생성'
categories:
  - java
tags:
  - java
  - jdbc
  - 코드모음
---

```java
import java.sql.Connection;
import java.sql.DriverManager;

public class DBConn {
    private static Connection conn = null;

    private DBConn() {
    }

    public static Connection getConnection() {
        String url = "jdbc:oracle:thin:@220.76.176.66:1521:orcl", user = "noritersand", pwd = "java301$!";
        if (conn == null) {
            try {
                Class.forName("oracle.jdbc.driver.OracleDriver");
                conn = DriverManager.getConnection(url, user, pwd);
            } catch (Exception e) {
                System.out.println(e);
            }
        }
        return conn;
    }

    public static void close() {
        if (conn != null) {
            try {
                if (!conn.isClosed())
                    conn.close();
            } catch (Exception e) {
                System.out.println(e);
            }
        }
        conn = null;
    }
}
```
