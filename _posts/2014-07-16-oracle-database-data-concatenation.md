---
layout: post
date: 2014-07-16 10:53:00 +0900
title: '[Oracle Database] data concatenation'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - concatenation
---

* Kramdown table of contents
{:toc .toc}

**data concatenation**: 한 건 이상의 데이터를 연결된 하나의 문자열로 출력하는 것.

가령 데이터가 다음과 같을 때:

```
ID              NAME
---------------------
noriter        놀이터
noriter        돌고래
noriter        글쓴이
```

ID가 'noriter'인 로우의 NAME을 하나의 문자열로 출력하려는 경우다.

```
NAMES
---------------------------
놀이터, 돌고래, 글쓴이
```


![](/images/oracle-data-concatenation.png)

테이블의 구조가 위와 같다고 하자. 그리고 우리가 원하는 것은:

- CHANNEL_MAPPING 테이블에서 매핑정보 조회
- 회원정보에는 해당 회원과 연결된 모든 채널번호를 포함.
- 반드시 한 회원당 하나의 레코드로 조회

일 때, 데이터베이스가 오라클이면 [LISTAGG](https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions089.htm#SQLRF30030)를 사용한다.

```
LISTAGG (MEASURE_EXPR, 'DELIMITER') WITHIN GROUP(ORDER_BY_CLAUSE)
~
OVER QUERY_PARTITION_CLAUSE
```

```sql
SELECT  mem_no,
        LISTAGG(chnl_no, ', ') WITHIN GROUP(ORDER BY chnl_no DESC) AS chnl_no
FROM    channel_mapping
GROUP BY mem_no
```

```
MEM_NO         CHNL_NO
-------------   -----------------------------------------------
926432104         10793, 10192, 10091, 10090
926432102         10891, 10090
926432101         10692, 10689, 10588
```

### example 1

```sql
SELECT department_id "Dept.",
    LISTAGG(last_name, '; ')
    WITHIN GROUP (ORDER BY hire_date) "Employees"
FROM employees
GROUP BY department_id
ORDER BY department_id
```

```
Dept. Employees
------ ------------------------------------------------------------
    10 Whalen
    20 Hartstein; Fay
    30 Raphaely; Khoo; Tobias; Baida; Himuro; Colmenares
    40 Mavris
    50 Kaufling; Ladwig; Rajs; Sarchand; Bell; Mallin; Weiss; Davie
       s; Marlow; Bull; Everett; Fripp; Chung; Nayer; Dilly; Bissot
       ; Vollman; Stiles; Atkinson; Taylor; Seo; Fleaur; Matos; Pat
       el; Walsh; Feeney; Dellinger; McCain; Vargas; Gates; Rogers;
        Mikkilineni; Landry; Cabrio; Jones; Olson; OConnell; Sulliv
       an; Mourgos; Gee; Perkins; Grant; Geoni; Philtanker; Markle
    60 Austin; Hunold; Pataballa; Lorentz; Ernst
    70 Baer
```

### example 2

```sql
SELECT department_id "Dept", hire_date "Date", last_name "Name",
    LISTAGG(last_name, '; ')
    WITHIN GROUP (ORDER BY hire_date, last_name)
    OVER (PARTITION BY department_id) as "Emp_list"
FROM employees
WHERE hire_date < '01-SEP-2003'
ORDER BY "Dept", "Date", "Name"
```

```
 Dept Date      Name              Emp_list
----- --------  ---------------  --------------------------------------------
   30 07-DEC-02  Raphaely       Raphaely; Khoo
   30 18-MAY-03  Khoo            Raphaely; Khoo
   40 07-JUN-02   Mavris          Mavris
   50 01-MAY-03  Kaufling         Kaufling; Ladwig
   50 14-JUL-03   Ladwig          Kaufling; Ladwig
   70 07-JUN-02   Baer             Baer
   90 13-JAN-01   De Haan        De Haan; King
   90 17-JUN-03   King             De Haan; King
  100 16-AUG-02  Faviet           Faviet; Greenberg
  100 17-AUG-02  Greenberg      Faviet; Greenberg
  110 07-JUN-02   Gietz            Gietz; Higgins
  110 07-JUN-02   Higgins         Gietz; Higgins
```
