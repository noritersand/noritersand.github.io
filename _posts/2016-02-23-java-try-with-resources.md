---
layout: post
date: 2016-02-23 15:48:00 +0900
title: 'Java: try with resources'
categories:
  - java
tags:
  - java
  - try-with-resources
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

#### since

- JDK7

```java
public class Connector {
    public static void main(String[] args) {
        try (Connection conn = getConnection()) {
            System.out.println("Got Connection.");
            Statement st = conn.createStatement();
            st = conn.createStatement();

            // 생략

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws Exception {
        String driver = "oracle.jdbc.driver.OracleDriver";
        String url = "jdbc:oracle:thin:@27.1.10.238:1521:icdb";
        String username = "icuser";
        String password = "icuser";
        Class.forName(driver); // load Oracle driver
        Connection conn = DriverManager.getConnection(url, username, password);
        return conn;
    }
}
```

위 코드의 conn 처럼 `try` 키워드 다음에 오는 괄호`()` 안에서 선언된 변수에 대해 close를 자동으로 수행한다. 그러니까 명시적으로 close를 하지 않아도 된다는 말이다. 단, 해당 인스턴스의 클래스가 AutoCloseable의 구현체일 때만 가능하다.

예를 들어 java.sql.Connection은 다음처럼 선언되어있고:

```java
public interface Connection extends Wrapper, AutoCloseable {
```

try-with-resources에 사용할 수 있다.

## example

```java
import java.net.URI;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class HttpUtil {
    public static String sendSimpleRequest(URI uri) throws Exception {
        HttpGet request = new HttpGet(uri);
        try (CloseableHttpClient client = HttpClients.createDefault();
                CloseableHttpResponse response = client.execute(request)) {
            HttpEntity entity = response.getEntity();
            if (entity != null) {
                String result = EntityUtils.toString(entity, "UTF-8");
                return result;
            }
            // client와 response는 자동으로 close 됨
        } catch (Exception e) {
            throw e;
        }
        return null;
    }
}
```
