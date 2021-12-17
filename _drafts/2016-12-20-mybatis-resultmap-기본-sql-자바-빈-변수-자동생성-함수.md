---
layout: post
date: 2016-12-20 20:55:00 +0900
title: '[Mybatis] 마이바티스용 resultmap, 기본 sql, 자바 빈 변수 자동생성 함수'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - resultmap
---

* Kramdown table of contents
{:toc .toc}

## resultmap 자동 생성

### 함수 정의

```sql
CREATE OR REPLACE FUNCTION MARS.FCOMM_GEN_RESULTMAP (
    p_table   IN VARCHAR2
) RETURN VARCHAR2 IS
    v_ret_val CLOB;
    v_work VARCHAR2(100);

   CURSOR SELS_CUR IS
        SELECT '    ' TXT, 0 ORD FROM DUAL UNION ALL
        SELECT  '        ' TXT, COLUMN_ID ORD
        FROM ALL_TAB_COLUMNS
        WHERE OWNER = 'MARS'
        AND TABLE_NAME = p_table UNION ALL
        SELECT '    ' TXT, 1000 ORD FROM DUAL
        ORDER BY ORD;
  SELS_REC SELS_CUR%ROWTYPE;

BEGIN
    IF p_table IS NULL THEN
        RETURN NULL;
    END IF;

    BEGIN
        SELECT LOWER(T2) INTO v_work
        FROM (
        SELECT 'TCMPY' T1, 'COMPANY' T2 FROM DUAL UNION ALL
        SELECT 'TCOMM' T1, 'COMMON' T2 FROM DUAL UNION ALL
        SELECT 'TWCRD' T1, 'WELFARECARD' T2 FROM DUAL UNION ALL
        SELECT 'TWPNT' T1, 'WELFAREPOINT' T2 FROM DUAL UNION ALL
        SELECT 'TCUST' T1, 'CUSTOMERCENTER' T2 FROM DUAL UNION ALL
        SELECT 'TPROD' T1, 'PRODUCT' T2 FROM DUAL UNION ALL
        SELECT 'TSVR' T1, 'SERVICE' T2 FROM DUAL UNION ALL
        SELECT 'TDISP' T1, 'DISPLAY' T2 FROM DUAL UNION ALL
        SELECT 'TVNDR' T1, 'VENDOR' T2 FROM DUAL UNION ALL
        SELECT 'TORD' T1, 'ORDER' T2 FROM DUAL UNION ALL
        SELECT 'TCLM' T1, 'CLAIM' T2 FROM DUAL UNION ALL
        SELECT 'TPATR' T1, 'PARTNER' T2 FROM DUAL UNION ALL
        SELECT 'TMBR' T1, 'MEMBER' T2 FROM DUAL UNION ALL
        SELECT 'TSBIZ' T1, 'SPECIALBUSINESS' T2 FROM DUAL)
        WHERE T1 = SUBSTR(p_table, 1,5)
        ;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'NOT DATA FOUND';
        WHEN OTHERS THEN
            RETURN 'OTHERS ERROR';
    END;

    v_ret_val := 'DBMS OUTPUT으로 보세요';
    BEGIN
        FOR SELS_REC IN SELS_CUR LOOP
             --v_ret_val := v_ret_val || SELS_REC.TXT||CHR(13)||CHR(10);
             dbms_output.put_line(SELS_REC.TXT);
        END LOOP;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'NOT DATA FOUND';
        WHEN OTHERS THEN
            RETURN 'OTHERS ERROR';
    END;

    RETURN v_ret_val;
END;
/
```

### 실행

```sql
SELECT FCOMM_GEN_RESULTMAP('테이블명') FROM DUAL;
```

## 기본 SQL 자동생성

### 함수 정의

```sql
CREATE OR REPLACE FUNCTION MARS.FCOMM_GEN_SQL (
    p_table   IN VARCHAR2
) RETURN VARCHAR2 IS
    v_ret_val VARCHAR2(4000);

   CURSOR SELS_CUR IS
        SELECT'    ' TXT, 1 ORD FROM DUAL UNION ALL
        SELECT '        /**/'||CHR(13)||CHR(10)||'        SELECT' TXT, 2 ORD FROM DUAL UNION ALL
        SELECT CASE WHEN COLUMN_ID = 1 THEN '               ' ELSE '             , ' END||SUBSTR(p_table, 7)||'.'||COLUMN_NAME TXT, COLUMN_ID+2 ORD
        FROM ALL_TAB_COLUMNS WHERE OWNER = 'MARS' AND TABLE_NAME = p_table UNION ALL
        SELECT '        FROM   '||p_table||' '||SUBSTR(p_table, 7) ||CHR(13)||CHR(10)||'        WHERE  1=1' TXT, 10001 ORD FROM DUAL UNION ALL
        SELECT '        '||CHR(13)||CHR(10)||'        AND    '||SUBSTR(p_table, 7)||'.'||COLUMN_NAME ||' = #{'
                    ||LOWER(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',1)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',2)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',3)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',4)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',5)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',6)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',7)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',8)))
                    ||'}'||CHR(13)||CHR(10)||'        ' TXT, COLUMN_ID+10002 ORD
       FROM ALL_TAB_COLUMNS
        WHERE OWNER = 'MARS' AND TABLE_NAME = p_table UNION ALL
        SELECT '        ORDER BY '||SUBSTR(p_table, 7)||'.'||COLUMN_NAME TXT, 20000 ORD FROM ALL_TAB_COLUMNS WHERE OWNER = 'MARS' AND TABLE_NAME = p_table AND COLUMN_ID = 1 UNION ALL
        SELECT '    ' TXT, 20001 ORD FROM DUAL
        ORDER BY ORD;
   SELS_REC SELS_CUR%ROWTYPE;


   CURSOR INS_CUR IS
        SELECT '    '||CHR(13)||CHR(10)||'        /**/'||CHR(13)||CHR(10)||'        INSERT INTO '||p_table||CHR(13)||CHR(10)||'             (' TXT, 0 ORD FROM DUAL UNION ALL
        SELECT CASE WHEN COLUMN_ID = 1 THEN '               ' ELSE '             , ' END||COLUMN_NAME TXT, COLUMN_ID ORD
        FROM ALL_TAB_COLUMNS WHERE OWNER = 'MARS' AND TABLE_NAME = p_table
        AND COLUMN_NAME NOT IN ('REGR_NO','REG_PGM_URL','REG_DTS','MODR_NO','MOD_PGM_URL','MOD_DTS') UNION ALL
        SELECT '             , '||CHR(13)||CHR(10)||'             )'||CHR(13)||CHR(10)||'        VALUES'||CHR(13)||CHR(10)||'             (' TXT, 10000 ORD FROM DUAL UNION ALL
        SELECT CASE WHEN COLUMN_ID = 1 THEN '               ' ELSE '             , ' END||'#{'
                    ||LOWER(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',1)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',2)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',3)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',4)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',5)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',6)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',7)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',8)))
                    ||'}' TXT, COLUMN_ID+20000 ORD
        FROM ALL_TAB_COLUMNS WHERE OWNER = 'MARS' AND TABLE_NAME = p_table
        AND COLUMN_NAME NOT IN ('REGR_NO','REG_PGM_URL','REG_DTS','MODR_NO','MOD_PGM_URL','MOD_DTS') UNION ALL
        SELECT '             ,  '||CHR(13)||CHR(10)||'             )'||CHR(13)||CHR(10)||'    ' TXT, 9999999 ORD FROM DUAL
        ORDER BY ORD
   ;

   INS_REC INS_CUR%ROWTYPE;

   CURSOR UPD_CUR IS
        SELECT '    '||CHR(13)||CHR(10)||'        /**/'||CHR(13)||CHR(10)||'        UPDATE '||p_table||CHR(13)||CHR(10)||'        SET ' TXT, 0 ORD FROM DUAL UNION ALL
        SELECT  '               '||''||CHR(13)||CHR(10)||
                    CASE WHEN COLUMN_ID = 1 THEN '               ' ELSE '             , ' END||SUBSTR(TABLE_NAME, 7)||'.'||COLUMN_NAME||' = #{'
                    ||LOWER(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',1)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',2)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',3)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',4)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',5)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',6)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',7)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',8)))
                    ||'}'||CHR(13)||CHR(10)||'               ' TXT, COLUMN_ID ORD
        FROM ALL_TAB_COLUMNS
        WHERE OWNER = 'MARS' AND TABLE_NAME = p_table UNION ALL
        SELECT '        WHERE  1=1' TXT, 10001 ORD FROM DUAL UNION ALL
        SELECT '        '||CHR(13)||CHR(10)||'        AND    '||SUBSTR(p_table, 7)||'.'||COLUMN_NAME ||' = #{'
                    ||LOWER(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',1)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',2)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',3)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',4)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',5)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',6)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',7)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',8)))
                    ||'}'||CHR(13)||CHR(10)||'        ' TXT, COLUMN_ID+10002 ORD
        FROM ALL_TAB_COLUMNS
        WHERE OWNER = 'MARS' AND TABLE_NAME = p_table UNION ALL
        SELECT '    ' TXT, 20001 ORD FROM DUAL
        ORDER BY ORD;

   UPD_REC UPD_CUR%ROWTYPE;

BEGIN
    IF p_table IS NULL THEN
        RETURN NULL;
    END IF;

    v_ret_val := 'DBMS OUTPUT으로 보세요';
    BEGIN
        FOR SELS_REC IN SELS_CUR LOOP
             --v_ret_val := CONCAT((v_ret_val), (SELS_REC.TXT||CHR(13)||CHR(10)));
             dbms_output.put_line(SELS_REC.TXT);
        END LOOP;

        dbms_output.put_line(CHR(13)||CHR(10)||CHR(13)||CHR(10));

        FOR UPD_REC IN UPD_CUR LOOP
             --v_ret_val := CONCAT((v_ret_val), (SELS_REC.TXT||CHR(13)||CHR(10)));
             dbms_output.put_line(UPD_REC.TXT);
        END LOOP;

        dbms_output.put_line(CHR(13)||CHR(10)||CHR(13)||CHR(10));

        FOR INS_REC IN INS_CUR LOOP
             --v_ret_val := CONCAT((v_ret_val), (SELS_REC.TXT||CHR(13)||CHR(10)));
             dbms_output.put_line(INS_REC.TXT);
        END LOOP;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'NOT DATA FOUND';
        WHEN OTHERS THEN
            RETURN 'OTHERS ERROR';
    END;

    RETURN v_ret_val;
END;
/
```

### 실행

```sql
SELECT FCOMM_GEN_SQL('테이블명') FROM DUAL;
```

## 자바빈 변수 자동생성

### 함수 정의

```sql
CREATE OR REPLACE FUNCTION MARS.FCOMM_GEN_MODEL (
    p_table   IN VARCHAR2
) RETURN VARCHAR2 IS
    v_ret_val CLOB;
    v_work VARCHAR2(100);

   CURSOR SELS_CUR IS
        SELECT 'private '||
                    CASE WHEN DATA_TYPE IN ('VARCHAR2',  'CLOB') THEN 'String '
                             WHEN DATA_TYPE = 'CHAR' THEN 'Character '
                             WHEN DATA_TYPE = 'NUMBER' AND NVL(DATA_SCALE,0) > 0 THEN 'BigDecimal '
                             WHEN DATA_TYPE = 'NUMBER' THEN 'Long '
                             WHEN DATA_TYPE = 'DATE' THEN 'Date '
                             ELSE 'String ' END
                    ||LOWER(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',1)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',2)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',3)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',4)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',5)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',6)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',7)))
                    ||INITCAP(FCOMM_GET_FULLNAME(FCOMM_SPLIT(COLUMN_NAME,'_',8)))
                    ||';' TXT, COLUMN_ID ORD
        FROM ALL_TAB_COLUMNS
        WHERE OWNER = 'MARS'
        AND TABLE_NAME = p_table
        ORDER BY ORD;
  SELS_REC SELS_CUR%ROWTYPE;

BEGIN
    IF p_table IS NULL THEN
        RETURN NULL;
    END IF;

    v_ret_val := 'DBMS OUTPUT으로 보세요';
    BEGIN
        FOR SELS_REC IN SELS_CUR LOOP
             --v_ret_val := v_ret_val || SELS_REC.TXT||CHR(13)||CHR(10);
             dbms_output.put_line(SELS_REC.TXT);
        END LOOP;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'NOT DATA FOUND';
        WHEN OTHERS THEN
            RETURN 'OTHERS ERROR';
    END;

    RETURN v_ret_val;
END;
/
```

### 실행

```sql
SELECT FCOMM_GEN_MODEL('테이블명') FROM DUAL;
```
