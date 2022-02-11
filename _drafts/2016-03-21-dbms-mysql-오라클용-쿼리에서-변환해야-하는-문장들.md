---
layout: post
date: 2016-03-21 00:00:00 +0900
title: '[DBMS] MariaDB, MySQL: 오라클용 쿼리에서 변환해야 하는 문장들'
categories:
  - dbms
tags:
  - dbms
  - mariadb
  - mysql
  - oracle
  - sql
---

* Kramdown table of contents
{:toc .toc}

## 개요

오라클에서 쓰던 쿼리를 그대로 MariDB, MySQL에서 돌리면 당연히 에러가 뻥뻥 터진다. 변환 작업 필요한 것들 함수, 문법 등 정리함.

## 변환 표

|오라클|MySQL|
|--|--|
|`SYSDATE`|`NOW()`|
|``` 'hi' \|\| 'there' ```|`CONCAT('hi', 'there')`|

## 테이블 별칭

- 오라클: 테이블 별칭을 쌍따옴표로 감싸기 가능. `AS` 키워드 사용 불가. ```FROM SOME_TABLE "SOME_ALIAS"```
- MySQL: 테이블 별칭을 쌍따옴표로 감싸기 불가. `AS` 키워드 사용 가능. ```FROM SOME_TABLE AS SOME_ALIAS```, ```FROM SOME_TABLE AS `SOME_ALIAS````

## null 처리

- 오라클: `NVL( expr1, expr2 )`
- MySQL: `IFNULL( expr1, expr2 )`
