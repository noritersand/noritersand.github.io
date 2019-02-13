---
layout: post
date: 2013-07-21 02:16:00 +0900
title: 'Oracle: tablespace, 테이블 스페이스'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - tablespace
---

* Kramdown table of contents
{:toc .toc}

테이블스페이스는 데이터베이스 오브젝트 내 실제 데이터를 저장하는 공간이다. 이것은 데이터베이스의 물리적인 부분이며, 세그먼트로 관리되는 모든 DBMS에 대해 저장소(세그먼트)를 할당한다.

데이터베이스 세그먼트는 데이터베이스 오브젝트 중의 하나이며, 테이블이나 인덱스와 같이 물리적 공간을 점유한다. 테이블스페이스는 한번 생성되면, 데이터베이스 세그먼트 생성시 이름으로 참조된다.

테이블스페이스는 단지 데이터베이스 저장소 위치를 지정할 뿐이며, 논리적 데이터베이스 구조나 스키마를 지정하지 않는다. 예를 들면, 동일한 스키마내의 다른 오브젝트는 서로 다른 테이블스페이스에 놓일 수 있다. 마찬가지로, 하나의 테이블스페이스는 여러 세그먼트들을 서비스 할 수 있다.

테이블스페이스를 사용하여 관리자는 디스크 배치를 통제할 수 있다. 데이터베이스의 일반적인 사용은 성능을 최적화 하는 것이다. 예를 들면, 빈번히 사용되는 인덱스는 빠른 SCSI 디스크에 놓을 수 있다. 반대로, 거의 사용되지 않는 데이터베이스 테이블은 값싸고 느린 IDE 디스크에 저장할 수 있다.

테이블스페이스가 파일시스템에 데이터를 저장하는 것이 일반적인 반면, 일부 DBMS는 raw device로 불리는 O/S device로 구성하여 운영 체제 파일시스템의 오버헤드를 없애고 더 빠른 성능을 제공한다.

#### 오라클에서 사용하는 논리적인 저장단위

- block: 데이터 입.출력 시 사용되는 운반단위. block의 종류는 2KB, 4KB, 8KB, 16KB, 32KB가 있다. 기본값은 8KB
- select: 데이터의 추출은 해당 데이터가 저장되어진 file(디스크)을 읽어 메모리에 로드시킨 후 CPU가 연산처리 해 결과를 보여준다.
- extent: 8개의 block이 모여서 1개의 extent를 이룬다. 이것은 세그먼트의 할당 단위이다. 즉, 테이블을 생성하면 기본적으로 디스크에 할당되는 크기는 1 extent가 된다.
- segment: 오라클에서 말하는 것으로 테이블과 인덱스의 집합.
- tablespace: 테이블(세그먼트)이 저장되는 공간을 말하는 것으로, 실제 하드디스크상에 저장되어진 물리적인 파일이름을 논리적으로 부를때 tablespace라고 부른다.
- database: 여러 종류의 테이블스페이스가 모여 이뤄진 것.

## 테이블스페이스 만들기

```
CREATE TABLESPACE [ tableSpaceName ]
DATAFILE [ 'directory\fileName.dbf ' ] SIZE [ size ]
AUTOEXTEND ON
MAXSIZE [ size ]
NEXT 8m
EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO
```

```sql
CREATE TABLESPACE test_tbs_a
DATAFILE 'c:\projectdata\test_tbs_a01.dbf ' SIZE 40m
EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO
```

## 작성한 테이블스페이스에 테이블 할당

```
CREATE 테이블 (
  ...
) TABLESPACE 테이블스페이스명
```

```sql
CREATE TABLE TBL_MEMBER_A (
  ID VARCHAR2(20),
  NAME VARCHAR2(10)
) TABLESPACE TEST_TBS_A;

SELECT TABLESPACE_NAME
FROM USER_TABLES
WHERE TABLE_NAME = 'TBL_MEMBER_B';  --혹은 SELECT * FROM user_tables
```

## 테이블스페이스 경로확인하기

```sql
SELECT * FROM DBA_DATA_FILES;
SELECT * FROM USER_TAB_COLUMNS WHERE TABLE_NAME = 'EMPLOYEES';
SELECT * FROM USER_TABLES WHERE TABLE_NAME = 'EMP';
```

예를 들어 scott 유저의 EMP 테이블의 테이블스페이스는 USERS 이며 `c:\oradata\orcl\users01.dbf` 로 저장되어있는걸 알 수 있다.

## 테이블스페이스의 남은 용량 조회

```sql
SELECT *
FROM dba_free_space
where tablespace_name = 'TEST_TBS_A';

SELECT *
FROM dba_tablespaces; --위 쿼리와 같이 비교해볼 것
```

## block_id란?

값이 9라면 block 번호 9번부터 사용가능, 이 말은 block 번호 1번부터 8번까지 사용중이라는 말. 즉, 위에서 생성한 테이블 스페이스가 8블록(64KB)을 파일헤더용으로 사용하고 있다는것을 볼 수 있음.

```
사용가능한 Bytes : 41877504  |  사용가능한 Blocks : 5112
```
