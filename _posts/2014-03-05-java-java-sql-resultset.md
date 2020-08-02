---
layout: post
date: 2014-03-05 14:21:00 +0900
title: '[Java] java.sql.ResultSet'
categories:
  - java
tags:
  - java
  - serialization
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [http://docs.oracle.com/javase/7/docs/api/java/sql/ResultSet.html](http://docs.oracle.com/javase/7/docs/api/java/sql/ResultSet.html)

## 주요 상수

- `CONCUR_READ_ONLY`: 읽기 전용으로 결과 데이터는 바꿀 수 없다.
- `CONCUR_UPDATABLE`: 결과 데이터를 갱신할 수 있다.
- `TYPE_FORWARD_ONLY`: 순방향 검색만 가능하다.
- `TYPE_SCROLL_INSENSITIVE`: 커서는 스크롤이 가능하지만 DB의 변화를 감지하지 못한다. 즉, 질의 문을 수행한 시점의 데이터를 얻을 수 있으며 이후 변화는 반영되지 않는다.
- `TYPE_SCROLL_SENSITIVE`: 커서는 스크롤이 가능하며 DB의 변화를 감지한다. 즉, 가장 최근에 바뀐 DB의 데이터를 얻을 수 있다.

## 주요 메서드

- `findColumn(String columnIndex)`: 열 이름과 관련된 열의 색인을 제공한다.
- `getByte(int columnIndex)`
- `getByte(String columnLabel)`
- `getDouble(int columnIndex)`
- `getDouble(String columnLabel)`: 열 이름 columnLabel이나 열 번호 columnIndex(1부터 시작)를 갖는 열의 값을 명시된 형으로 변환하고 이를 반환한다.
- `getObject(int columnIndex)`
- `getObject(String columnName)`: 주어진 컬럼에 해당하는 데이터를 해당하는 타입에 대응하는 객체로 반환한다.
- `close()`: 현재 결과 집합을 즉시 닫는다.
- `next()`: 다음 레코드로 이동 한다.

## 커서 관련 메서드

- `first()`: 첫 번째 레코드로 이동
- `previous()`: 이전 레코드로 이동
- `next()`: 다음 레코드로 이동
- `last()`: 마지막 레코드로 이동
- `absolute(int row)`: 주어진 열로 커서를 이동시킨다.
- `afterLast()`: 마지막의 다음 열로 커서를 이동시킨다.
- `beforeFirst()`: 처음의 이전 열로 커서를 이동시킨다.
- `getRow()`: 현재 커서가 가리키는 열 번호를 돌려준다.
- `moveToCurrentRow()`: 커서를 이전에 기억된 위치로 이동. `moveToInsertRow()` 메서드를 사용해서 삽입 열로 이동한 후
에 복귀할 때 사용할 수 있다.
- `moveToInsertRow()`: 커서를 삽입 열로 이동
- `relative(int rows)`: 현재 위치에서 주어진 양만큼 상대적인 위치로 커서를 이동

## example

#### 쿼리 결과값 처리

```java
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, hak);
ResultSet rs = pstmt.executeQuery();

if (rs.next()) { // ResultSet의 "커서"를 BOF에서 첫번째 레코드로 이동
    dto.setHak(rs.getString("hak"));
    dto.setName(rs.getString("name"));
    dto.setBirth(rs.getString("birth"));
    dto.setKor(rs.getInt("kor"));
    dto.setEng(rs.getInt("eng"));
    dto.setAvg((rs.getDouble("kor") + rs.getDouble("eng")) / 3);
} else
    System.out.println("해당 학번은 없습니다.");

pstmt.close();
rs.close();
```

#### 스크롤

```java
char ch = (char) System.in.read();
System.in.skip(2);
switch (ch) {
case '1':
    if (rs.first()) {
        System.out.println("다음: " + rs.getString(1) + ":" + rs.getString(2));
    }
case '2':
    if (rs.previous()) {
        System.out.println("다음: " + rs.getString(1) + ":" + rs.getString(2));
    }
case '3':
    if (rs.next()) {
        System.out.println("다음: " + rs.getString(1) + ":" + rs.getString(2));
    }
case '4':
    if (rs.last()) {
        System.out.println("다음: " + rs.getString(1) + ":" + rs.getString(2));
    }
case '5':
    DBConn.close();
    System.exit(0);
    break;
}
```
