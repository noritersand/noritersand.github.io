---
layout: post
date: 2014-07-21 18:44:00 +0900
title: '[Oracle Database] 재귀 구조를 갖는 데이터의 경로 출력'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - hierarchy
---

* Kramdown table of contents
{:toc .toc}

```sql
CREATE OR REPLACE FUNCTION DEV_IMK.FN_GET_DISP_FULLNM (
    ARG_DISP_NO DISPLAY_CLASS.DISP_NO%TYPE,
    ARG_DELIMITER VARCHAR2
)
RETURN VARCHAR2 IS
    V_DISP_NM   VARCHAR2(400);

BEGIN
   SELECT   MAX(CASE WHEN DISP_DEP = 1 THEN DISP_NM END) ||
            MAX(CASE WHEN DISP_DEP = 2 THEN ARG_DELIMITER || DISP_NM END) ||
            MAX(CASE WHEN DISP_DEP = 3 THEN ARG_DELIMITER || DISP_NM END) ||
            MAX(CASE WHEN DISP_DEP = 4 THEN ARG_DELIMITER || DISP_NM END) ||
            MAX(CASE WHEN DISP_DEP = 5 THEN ARG_DELIMITER || DISP_NM END) ||
            MAX(CASE WHEN DISP_DEP = 6 THEN ARG_DELIMITER || DISP_NM END)
            INTO V_DISP_NM
   FROM     (
            SELECT   DISP_NO, DISP_NM, HIGH_DISP_NO, DISP_DEP
            FROM     DISPLAY_CLASS
            WHERE    SHOP_NO = ARG_SHOP_NO
            AND      DISP_NO LIKE SUBSTR(ARG_DISP_NO, 1, 3) || '%' -- 1DEPTH 이하모든전시
            )
   START WITH DISP_NO = ARG_DISP_NO
   CONNECT BY DISP_NO = PRIOR HIGH_DISP_NO
   ORDER BY DISP_NO;

   RETURN V_DISP_NM;

   EXCEPTION
       WHEN  NO_DATA_FOUND THEN NULL;
       WHEN  OTHERS THEN RAISE; -- Consider logging the error and then re-raise
END FN_GET_DISP_FULLNM;
/
```

![](/images/oracle-hierarchy-1.png)

위와 같이 자기 자신을 참조하는 데이터 구조일 때 (보통 분류를 다루는 테이블에서 많이 볼 수 있다.) 하위 데이터를 기준으로 상위 데이터의 경로를 출력하는 프로시저.

가령 데이터가 아래와 같을 때는:

```sql
DISP_NO        HIGH_DISP_NO    DISP_DEPTH    DISP_NM
-------        ------------    ----------    --------------
101            0               1             최상위
10101          101             2             그다음
1010101        10101           3             그다음다음
```

이렇게 된다:

```sql
SELECT  FN_GET_DISP_FULLNM('1010101', ' > ')
FROM    DUAL

-> 최상위 > 그다음 > 그다음다음
```
