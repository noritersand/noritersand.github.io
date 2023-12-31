---
layout: post
date: 2014-10-29 18:32:00 +0900
title: '[Oracle Database] 다중 행을 하나의 행으로 출력'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - split
---

* Kramdown table of contents
{:toc .toc}

다중 행을 하나의 행으로 출력한다. 하나의 컬럼으로 합쳐서 구분자로 표현하는 것이 아니라, 아예 다수의 컬럼으로 쪼개는 방법이다.

이게 무슨말이냐면:

```sql
WITH temp1 AS (
    SELECT 1001 AS mem_no, '김모양' AS mem_nm FROM dual
),
temp2 AS (
    SELECT 1001 AS mem_no, '01' AS tp, '01001' AS tp_value FROM dual
    UNION
    SELECT 1001 AS mem_no, '02' AS tp, '02002' AS tp_value FROM dual
    UNION
    SELECT 1001 AS mem_no, '03' AS tp, '03003' AS tp_value FROM dual
)
SELECT temp1.mem_no, mem_nm, tp, tp_value
FROM temp1
JOIN temp2 ON temp1.mem_no = temp2.mem_no
```

![](/images/oracle-concat-and-split-1.png)

위와 같은 데이터가 있을 때 아래처럼 출력하려고 하는 방법이다.

![](/images/oracle-concat-and-split-2.png)

### \#1

우선 무식한 방법으로는:

```sql
SELECT temp1.mem_no, mem_nm,
    tp01.tp_value AS tp01,
    tp02.tp_value AS tp02,
    tp03.tp_value AS tp03
FROM temp1
JOIN temp2 tp01 ON temp1.mem_no = tp01.mem_no AND tp01.tp = '01'
JOIN temp2 tp02 ON temp1.mem_no = tp02.mem_no AND tp02.tp = '02'
JOIN temp2 tp03 ON temp1.mem_no = tp03.mem_no AND tp03.tp = '03'
```

### \#2

위보단 덜 무식한 방법으로 스칼라 쿼리를 이용할 경우:

SELECT temp1.mem_no, mem_nm,
    (SELECT tp_value FROM temp2 WHERE tp = '01') AS tp01,
    (SELECT tp_value FROM temp2 WHERE tp = '02') AS tp02,
    (SELECT tp_value FROM temp2 WHERE tp = '03') AS tp03
FROM temp1


### \#3

이제 조금 우아하게 CASE문으로 ALIAS를 생성한 뒤 이것을 뷰로 사용하여 GROUP BY를 적용하는 방법이 있다:

```sql
SELECT mem_no, mem_nm, MAX(tp01), MAX(tp02), MAX(tp03)
FROM (
    SELECT temp1.mem_no, mem_nm,
        CASE WHEN TP ='01' THEN tp_value ELSE NULL END AS tp01,
        CASE WHEN TP ='02' THEN tp_value ELSE NULL END AS tp02,
        CASE WHEN TP ='03' THEN tp_value ELSE NULL END AS tp03
    FROM temp1
    JOIN temp2 ON temp1.mem_no = temp2.mem_no
) GROUP BY mem_no, mem_nm
```

단, 이 방법은 최종적으로 출력해야 하는 컬럼의 수가 늘어날 수록 GROUP BY로 포함해야 하는 컬럼 또한 같이 늘어난다는 단점이 있다. 따라서 출력하는 컬럼이 속도에 영향을 줄 정도로 증가한다면 다음과 같은 방법이 고려될 수 있다:

```sql
SELECT a.mem_no, mem_nm, tp01, tp02, tp03
FROM temp1 a
JOIN (
    SELECT mem_no, MAX(tp01) AS tp01, MAX(tp02) AS tp02, MAX(tp03) AS tp03
    FROM (
        SELECT mem_no,
            CASE WHEN TP ='01' THEN tp_value ELSE NULL END AS tp01,
            CASE WHEN TP ='02' THEN tp_value ELSE NULL END AS tp02,
            CASE WHEN TP ='03' THEN tp_value ELSE NULL END AS tp03
        FROM temp2
    )
    GROUP BY mem_no
) b ON a.mem_no = b.mem_no
```
