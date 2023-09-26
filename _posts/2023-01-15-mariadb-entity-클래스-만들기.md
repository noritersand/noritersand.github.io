---
layout: post
date: 2023-01-15 14:00:00 +0900
title: '[MariaDB] Entity 클래스 만들기'
categories:
  - mariadb
tags:
  - mariadb
  - dbms
  - sql
  - rdb
  - entity
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](somewhere)

#### 테스트 환경 정보

- MariaDB 10.5.17
- MySQL 8.0.25


## Entity 클래스 만들기

```sql
/* 테이블 + 컬럼 코멘트 */
set @table_name = 'table_name';
set @table_schema = 'schema_name';

select 0 as ordinal_position, 'it\'s table' as column_name, '' as column_type, concat('/**', table_name, ' ', table_comment, ' 테이블 클래스', '*/\r\r') as str
from information_schema.tables
where table_name = @table_name
and table_schema = @table_schema
union
select ordinal_position, column_name, column_type, concat('\t/**', column_comment, '*/\r\tprivate ',
  case
    when column_type like 'int%' then 'Integer'
    when column_type like 'varchar%' then 'String'
    when column_type like 'datetime%' then 'java.time.LocalDateTime'
    when column_type like 'date%' then 'java.time.LocalDate'
    when column_type like 'time%' then 'java.time.LocalTime'
    when column_type like 'tinyint%' then 'Integer'
    when column_type like 'smallint%' then 'Integer'
    when column_type like 'mediumint%' then 'Integer'
    when column_type like 'bigint%' then 'Long'
    when column_type like 'float%' then 'Float'
    when column_type like 'double%' then 'Double'
    when column_type like 'decimal%' then 'BigDecimal'
    when column_type like 'text%' then 'String'
    when column_type like 'blob%' then 'String'
    when column_type like 'binary%' then 'String'
    when column_type like 'char%' then 'String'
    when column_type like 'enum%' then 'String'
    when column_type like 'set%' then 'String'
    when column_type like 'bool%' then 'Boolean'
    when column_type like 'boolean%' then 'Boolean'
    when column_type like 'tinyblob%' then 'String'
    when column_type like 'tinytext%' then 'String'
    when column_type like 'mediumblob%' then 'String'
    when column_type like 'mediumtext%' then 'String'
    when column_type like 'longblob%' then 'String'
    when column_type like 'longtext%' then 'String'
  end
  , ' ', column_name, ';') as str
from information_schema.columns
where table_name = @table_name
and table_schema = @table_schema
order by ordinal_position asc
```

`set` 까지 한 번에 실행하면 된다. 결과에서 `str` 컬럼 전체를 Java 클래스에 붙여넣으면 끗. DBMS 툴에 따라 큰따옴표가 붙을 수 있는데 그냥 전부 지우면 됨.
