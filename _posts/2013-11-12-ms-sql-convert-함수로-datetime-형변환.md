---
layout: post
date: 2013-11-12 12:27:00 +0900
title: '[MS-SQL] CONVERT 함수로 DATETIME 형변환'
categories:
  - mssql
tags:
  - dbms
  - mssql
  - convert
  - datetime
---

* Kramdown table of contents
{:toc .toc}

```
CONVERT( data_type [( length )] , expression [, style ])
```

```sql
SELECT
  GETDATE() AS ex1,
  CONVERT(VARCHAR, GETDATE(), 120) AS ex2
FROM dbo.bod_faq

ex1
-----------------------
2013-11-11 18:45:28.560

ex2
-----------------------
2013-11-11 18:45:28
```

#### 왜 썼는지 기억 안나는 문단

org.apache.ibatis.session.SqlSession을 이용한 JDBC의 경우:

MS-SQL의 DATETIME은 java.sql.Timestamp 클래스 타입으로 반환한다.
String으로 받고싶다면 CONVERT()를 통해 VARCHAR 타입으로 변환한다.

#### style 표

| 번호 | 출력값                      | 사용방법                                   |
|------|-----------------------------|--------------------------------------------|
| 0    |  Feb 22 2006 4:26PM         |  CONVERT(CHAR(19), CURRENT_TIMESTAMP, 0)   |
| 1    |  02/22/06                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 1)    |
| 2    |  06.02.22                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 2)    |
| 3    |  22/02/06                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 3)    |
| 4    |  22.02.06                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 4)    |
| 5    |  22-02-06                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 5)    |
| 6    |  22 Feb 06                  |  CONVERT(CHAR(9), CURRENT_TIMESTAMP, 6)    |
| 7    |  Feb 22, 06                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 7)   |
| 8    |  16:26:08                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 8)    |
| 9    |  Feb 22 2006 4:26:08:020PM  |  CONVERT(CHAR(26), CURRENT_TIMESTAMP, 9)   |
| 10   |  02-22-06                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 10)   |
| 11   |  06/02/22                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 11)   |
| 12   |  060222                     |  CONVERT(CHAR(6), CURRENT_TIMESTAMP, 12)   |
| 13   |  22 Feb 2006 16:26:08:020   |  CONVERT(CHAR(24), CURRENT_TIMESTAMP, 13)  |
| 14   |  16:26:08:037               |  CONVERT(CHAR(12), CURRENT_TIMESTAMP, 14)  |
| 20   |  2006-02-22 16:26:08        |  CONVERT(CHAR(19), CURRENT_TIMESTAMP, 20)  |
| 21   |  2006-02-22 16:26:08.037    |  CONVERT(CHAR(23), CURRENT_TIMESTAMP, 21)  |
| 22   |  02/22/06 4:26:08 PM        |  CONVERT(CHAR(20), CURRENT_TIMESTAMP, 22)  |
| 23   |  2006-02-22                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 23)  |
| 24   |  16:26:08                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 24)   |
| 25   |  2006-02-22 16:26:08.037    |  CONVERT(CHAR(23), CURRENT_TIMESTAMP, 25)  |
| 100  |  Feb 22 2006 4:26PM         |  CONVERT(CHAR(19), CURRENT_TIMESTAMP, 100) |
| 101  |  02/22/2006                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 101) |
| 102  |  2006.02.22                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 102) |
| 103  |  22/02/2006                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 103) |
| 104  |  22.02.2006                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 104) |
| 105  |  22-02-2006                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 105) |
| 106  |  22 Feb 2006                |  CONVERT(CHAR(11), CURRENT_TIMESTAMP, 106) |
| 107  |  Feb 22, 2006               |  CONVERT(CHAR(12), CURRENT_TIMESTAMP, 107) |
| 108  |  16:26:08                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 108)  |
| 109  |  Feb 22 2006 4:26:08:067PM  |  CONVERT(CHAR(26), CURRENT_TIMESTAMP, 109) |
| 110  |  02-22-2006                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 110) |
| 111  |  2006/02/22                 |  CONVERT(CHAR(10), CURRENT_TIMESTAMP, 111) |
| 112  |  20060222                   |  CONVERT(CHAR(8), CURRENT_TIMESTAMP, 112)  |
| 113  |  22 Feb 2006 16:26:08:067   |  CONVERT(CHAR(24), CURRENT_TIMESTAMP, 113) |
| 114  |  16:26:08:067               |  CONVERT(CHAR(12), CURRENT_TIMESTAMP, 114) |
| 120  |  2006-02-22 16:26:08        |  CONVERT(CHAR(19), CURRENT_TIMESTAMP, 120) |
| 121  |  2006-02-22 16:26:08.080    |  CONVERT(CHAR(23), CURRENT_TIMESTAMP, 121) |
| 126  |  2006-02-22T16:26:08.080    |  CONVERT(CHAR(23), CURRENT_TIMESTAMP, 126) |
| 127  |  2006-02-22T16:26:08.080    |  CONVERT(CHAR(23), CURRENT_TIMESTAMP, 127) |
| 130  |  24 ???? 1427 4:26:08:080PM |  CONVERT(CHAR(32), CURRENT_TIMESTAMP, 130) |
| 131  |  24/01/1427 4:26:08:080PM   |  CONVERT(CHAR(25), CURRENT_TIMESTAMP, 131) |
