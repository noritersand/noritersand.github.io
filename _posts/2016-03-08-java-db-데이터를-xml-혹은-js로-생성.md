---
layout: post
date: 2016-03-08 19:44:00 +0900
title: '[Java] DriverManager 사용 예시'
categories:
  - java
tags:
  - java
  - drivermanager
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

### example: DB 데이터를 XML 혹은 JS 파일로 생성

```java
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.Statement;
import java.util.Properties;

import com.google.gson.Gson;

public class DictionaryToXml {
    private static String path = "C:/project/workspace/test/WebContent/msg/";

    public static void main(String[] args) {
        try (Connection conn = getConnection()) {
            System.out.println("Got Connection.");
            Statement st = conn.createStatement();
            st = conn.createStatement();
            String sql = getSqlDictionary();
            ResultSet rs = st.executeQuery(sql);
            ResultSetMetaData rsMetaData = rs.getMetaData();

            int numberOfColumns = rsMetaData.getColumnCount();
            System.out.println("resultSet MetaData column Count=" + numberOfColumns);

            Properties propsKr = new Properties();
            while (rs.next()) {
                propsKr.setProperty(rs.getString("dictionaryId"), rs.getString("langKr"));
            }

            // export to xml
            OutputStream osKr = new FileOutputStream(path + "messages-ko.xml");
            propsKr.storeToXML(osKr, "dictionary KR");

            // export to js
            String scriptKr = "var messageDictionary = " + new Gson().toJson(propsKr)+";";
            Files.write(Paths.get(path + "messages-ko.js"), scriptKr.getBytes("UTF-8"));

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws Exception {
        String driver = "oracle.jdbc.driver.OracleDriver";
        String url = "jdbc:oracle:thin:@127.0.0.1:1521:mydb";
        String username = "test";
        String password = "test";

        Class.forName(driver); // load Oracle driver
        Connection conn = DriverManager.getConnection(url, username, password);
        return conn;
    }

    private static String getSqlDictionary() {
        StringBuilder sb = new StringBuilder();
        sb.append("SELECT DIC_CD as \"dictionaryId\",     ");
        sb.append("       CD_KOR_NM as \"langKr\",           ");
        sb.append("       CD_DESC as \"description\"      ");
        sb.append("  FROM AD_MLANG_DIC     ");
        sb.append(" ORDER BY DIC_CD ASC             ");
        return sb.toString();
    }
}
```
