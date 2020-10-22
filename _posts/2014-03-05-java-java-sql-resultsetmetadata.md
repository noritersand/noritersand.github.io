---
layout: post
date: 2014-03-05 14:21:00 +0900
title: '[Java] java.sql.ResultSetMetaData'
categories:
  - java
tags:
  - java
  - resultsetmetadata
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://docs.oracle.com/javase/7/docs/api/java/sql/ResultSetMetaData.html](http://docs.oracle.com/javase/7/docs/api/java/sql/ResultSetMetaData.html)

메타 데이터란 저장된 데이터 그 자체는 아니지만, 해당 데이터에 대한 정보를 갖고 있는 데이터를 의미한다. 즉, DB 내의 데이터에 대한 데이터의 소유, 데이터의 크기에 관련된 정보들이다.

ResultSetMetaData는 SQL로 받아온 데이터의 정보를 조회/출력하는 용도로 사용된다. ResultSet 인터페이스 객체의 `getMetaData()` 를 호출하여 ResultSetMetaData 인터페이스 객체를 얻으면 해당 ResultSet과 관련된 메타 데이터를 얻을 수 있다.

```java
String strQuery = "Select 필드_명  FROM 테이블_명";

ResultSet rs = stmt.executeQuery(strQuery); // stmt : Statement 객체
ResultSetMetaData resultMetaData = rs.getMetaData();
int cols = resultMetaData.getColumnCount(); // 필드 수 구하기
```

## 주요 메서드

- `getCatalogName(int column)`: 주어진 컬럼의 이름을 돌려준다.
- `getColumnClassName(int column)`: 주어진 컬럼의 객체를 저장할 때 사용되는 자바 클래스 이름을 돌려준다.
- `getColumnCount()`: 컬럼의 개수를 돌려준다.
- `getColumnDisplaySize(int column)`: 주어진 컬럼의 최대 문자 개수를 돌려준다.
- `getColumnLabel(int column)`: 주어진 컬럼의 권장 타이틀을 돌려준다.
- `getColumnName(int column)`: 주어진 컬럼의 이름을 돌려준다.
- `getColumnType(int column)`: 주어진 컬럼의  SQL 타입을 돌려준다. 반환하는 값은  java.sql.Types에 정의된 상수이다.
- `getColumnTypeName(int column)`: 주어진 컬럼의 DB에 종속된 이름을 돌려준다.
- `getTableName(int column)`: 주어진 컬럼이 속한 테이블 이름을 돌려준다.
- `isReadOnly(int column)`: 주어진 컬럼이 읽기 전용인지 알려준다.

## example

#### 쿼리 결과의 메타데이터 출력

```java
String sql = "select * from SCORE";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

ResultSetMetaData rsmd = rs.getMetaData();

int cols = rsmd.getColumnCount(); //컬럼의 총개수

for (int i = 1; i <= cols; i++) {
    System.out.print(rsmd.getColumnName(i) + "\t"); //컬럼명 출력
}

while (rs.next()) {
    for (int i = 1; i <= cols; i++) {
        System.out.print(rs.getString(i) + "\t");
    }
}
```
