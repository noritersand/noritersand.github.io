---
layout: post
date: 2022-01-07 18:28:23 +0900
title: '[DBMS] MariaDB 메모'
categories:
  - dbms
tags:
  - dbms
  - mariadb
  - sql
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://mariadb.com/kb/en/](https://mariadb.com/kb/en/)
- [https://dbschema.com/documentation/MariaDb/#introduction](https://dbschema.com/documentation/MariaDb/#introduction)

## 개요

MariaDB 관련 내용 메모.

## 로컬 서버 설정

### root로 접속

설치 경로에서 `mariadb.exe -u root -p` 실행:

```bash
PS C:\Program Files\MariaDB 10.7\bin> .\mariadb.exe -u root -p
```

혹은 같이 설치된 MySQL Client 실행.

### 데이터베이스 생성/보기/선택

```sql
-- 현재 상태 보기
status

CREATE DATABASE test;
```

```sql
-- 데이터베이스 조회
SHOW DATABASES;

-- 현재 데이터베이스의 모든 테이블 보기
SHOW TABLES;
```

```sql
-- 'mysql' 데이터베이스 사용
use mysql
```

### 로컬 접속용 유저 생성과 모든 권한 부여

```sql
CREATE USER 'fixalot'@'localhost' IDENTIFIED BY '1123';
GRANT ALL PRIVILEGES ON *.* TO 'fixalot'@'localhost';
FLUSH PRIVILEGES;
```

```sql
-- 부여 가능한 권한 보기
SHOW PRIVILEGES

-- fixalot@localhost의 부여된 권한 보기
SHOW GRANTS FOR fixalot@localhost;
```

## 로우 넘버

ROWNUM 출력 방법

```sql
SELECT @ROWNUM := @ROWNUM + 1 AS ROWNUM
FROM SAMPLE, (SELECT @ROWNUM := 0) R
```
