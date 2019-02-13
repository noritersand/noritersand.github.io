---
layout: post
date: 2013-07-01 01:00:00 +0900
title: 'Oracle: sys, etc'
categories:
  - oracle
tags:
  - oracle
  - dbms
  - sys
---

* Kramdown table of contents
{:toc .toc}

## 관리자 페이지

- Oracle Enterprise Manager 데이터베이스 컨트롤 URL: http://ADMIN:1158/em (em 실행(관리자 영역) / ADMIN은 사용자계정 혹은 IP)
- iSQL Plus URL: http://ADMIN:5560/isqlplus  (iSQL Plus 실행)
- iSQL Plus DBA URL: http://ADMIN:5560/isqlplus/dba

## 서버 시작/종료

```sql
-- 오라클 인스턴스 종료, DB마운트 해제
SQL> shutdown immediate

-- 오라클 인스턴스 시작, DB마운트
SQL> startup
```

휴지인스턴스 문제 발생 시 startup 혹은 윈도우 서비스에서 해당 항목을 시작한다

## 리스너 시작/종료

```bash
# 리스너컨트롤 시작하기
C:\>lsnrctl

# 리스너 관련 명령어들
LSNRCTL> start
LSNRCTL> stop
LSNRCTL> reload
LSNRCTL> status
LSNRCTL> help

# 혹은

c:\> lsnrctl start # start | stop | status

# SID 확인
c:\> lsnrctl services
```

## 로그인/로그오프/콘솔

```sql
console> sqlplus 계정명/암호
console> sqlplus / as sysdba

-- tnsnames.ora 에 정의된 connect_identifier 으로 접속
CMD> sqlplus 사용자명/"암호"@orcl

-- SQLPLUS연결을 유지한체 운영체제의 콘솔로 빠져나간다. disconnet와 다름
SQL> host

-- 콘솔빠져나가기(to sqlplus)
console> exit

-- 리눅스나 유닉스라면 ! cls
SQL> host cls

-- 로그오프
SQL> disconn

-- 로그인
SQL> conn sys/[PASSWORD] as sysdba
```

## 계정 생성/삭제

```sql
SQL> CREATE USER [user_name]
  IDENTIFIED BY [password]
  DEFAULT TABLESPACE [tablespace_name]
  TEMPORARY TABLESPACE TEMP;

ex) CREATE USER nextree IDENTIFIED BY nextree DEFAULT TABLESPACE NEXTREE TEMPORARY TABLESPACE TEMP;

CREATE USER [아이디] IDENTIFIED BY [비밀번호];

-- 계정삭제
DROP USER [아이디] CASCADE;

-- 생성한 USER로 ORACLE에 접속하기
sqlplus nextree/nextree[@db_sid]

-- Oracle db xe버전에서 scott 계정 불러오기
sqlplus> @C:\oraclexe\app\oracle\product\11.2.0\server\RDBMS\ADMIN\scott.sql
```

## 파일관리

```sql
-- sqlplus 에서 *.sql 파일 실행. 파일이 존재하는 경로에서 sqlplus를 실행하고, "@파일명" 명령을 실행하면 된다.
SQL>@ex.sql

-- 경로를 지정하여 실행
SQL>@d:\sql\ex.sql

-- sqlplus 에서 sql 작업 내용 자동 저장하기
SQL>spool d:\sql\test.txt

-- d:\sql\test.txt 파일 생성
SQL>SELECT * FROM insa;

-- 스풀 정지
SQL>spool off
```

## 조회

```sql
-- SID 확인
SQL> select instance from v$thread

-- 접속된 user의 모든 테이블 검색
SQL> SELECT * FROM TAB;         
SQL> DESC[RIBE] insa;
SQL> select * from v$pwfile_users;
SQL> select * from all_users;

-- sysdba, sys는 데이터베이스의 모든 권한을 갖는 관리자 계정을 말한다.
-- sysoper, system은 운영자를 의미하며 오라클서버의 on/off 조작만 가능하다.

-- 사용자 확인(sys 계정)
SELECT username, password, created FROM DBA_USERS;
SELECT username, password, created FROM DBA_USERS WHERE username='SCOTT';

-- 테이블 컬럼 확인
SELECT * FROM cols WHERE table_name = '테이블명';
SELECT * FROM user_tab_columns WHERE table_name='테이블명';
SELECT * FROM all_tab_columns WHERE owner ='디비계정' and table_name = '테이블명'

-- 코멘트 확인
SELECT * from all_tab_comments; -- 테이블 코멘트
SELECT * from all_col_comments; -- 컬럼 코멘트

-- 제약조건 확인
DESC user_constraints; --user_constraints 구조 확인
SELECT * FROM USER_constraints WHERE table_name='INSA';
-- 어떤 컬럼에 제약조건이 부여되었는지는 확인할 수 없다.
-- P : 기본키, C : NOT NULL 등, U : UNIQUE, R : 참조키 등
SELECT constraint_name, table_name, constraint_type FROM user_constraints;

-- 현재 user가 가지고 있는 column에 할당된 제약조건에 대한 정보
-- 어떤 컬럼에 기본키가 부여되었는지 확인 가능
SELECT * FROM USER_cons_columns;

-- procedure, function 목록 확인
SELECT object_name FROM user_procedures;

-- 테이블의 프로시저, 함수, 패키지 등 상호 참조되는 관계 확인
SELECT * FROM user_dependencies;

-- 트리거 확인
SELECT trigger_name, trigger_type, table_name FROM user_triggers;

-- 프로시저, 함수등의 소스 확인
SELECT TEXT FROM user_source;
SELECT * FROM user_source WHERE TYPE = 'PROCEDURE' AND NAME = 'PCD_EMPLOYEE_1';

-- 뷰 목록, 소스 확인
SELECT view_name, text FROM user_views;

-- 시퀀스 목록 확인
SELECT * FROM seq;
SELECT * FROM user_sequences;
```

## 권한 ROLE

- `CONNECT ROLE`: 오라클에 접속 할 수 있는 세션 생성 및 테이블을 생성하거나 조회 할 수 있는 가장 일반적인 권한들로 이루어져 있다. CONNECT ROLE이 없으면 유저를 생성하고서도 Oracle에 접속 할 수가 없다.
- `RESOURCE ROLE`: Store Procedure나 Trigger와 같은 PL/SQL을 사용할 수 있는 권한 들로 이루어져 있다. PL/SQL을 사용하려면 RESOURCE ROLE을 부여해야 한다. 유저를 생성하면 일반적으로 CONNECT, RESOURCE롤을 부여 한다.
- `DBA ROLE`: 모든 시스템 권한이 부여된 ROLE 이다. DBA ROLE은 데이터베이스 관리자에게만 부여해야 한다.

```bash
SQL> CREATE USER id IDENTIFIED BY "password";
SQL> GRANT CREATE SESSION TO id;
SQL> GRANT CREATE TABLE TO id;
SQL> GRANT CONNECT, RESOURCE TO id;
SQL> grant create view to scott;
SQL> alter user scott account unlock; # scott 잠금해제
SQL> alter user scott account lock; # scottt 잠금
SQL> alter user scott account unlock;
SQL> alter user scott account lock;
SQL> alter user scott identified by 1234;
SQL> alter user hr identified by 1234 account unlock;
SQL> grant sysdba to scott;
SQL> grant sysoper to scott;
SQL> revoke sysdba from scott;

GRANT CREATE SESSION TO 유저명 # 데이터베이스에 접근할 수 있는 권한
GRANT CREATE DATABASE LINK TO 유저명
GRANT CREATE MATERIALIZED VIEW TO 유저명
GRANT CREATE PROCEDURE TO 유저명
GRANT CREATE PUBLIC SYNONYM TO 유저명
GRANT CREATE ROLE TO 유저명
GRANT CREATE SEQUENCE TO 유저명
GRANT CREATE SYNONYM TO 유저명
GRANT CREATE TABLE TO 유저명 # 테이블을 생성할 수 있는 권한
GRANT DROP ANY TABLE TO 유저명 # 테이블을 제거할 수 있는 권한
GRANT CREATE TRIGGER TO 유저명
GRANT CREATE TYPE TO 유저명
GRANT CREATE VIEW TO 유저명
GRANT CREATE SESSION ,CREATE TABLE ,CREATE SEQUENCE ,CREATE VIEW TO 유저명;
GRANT [role1] [,role2] [,role3] [,...] TO [user_name];
GRANT connect, resource, dba TO kh301402;
```
