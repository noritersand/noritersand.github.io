---
layout: post
date: 2018-02-05 15:25:02 +0900
title: '[Oracle Database] 프로시저 호출방법'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - sql
---

이런 프로시저가 있을 때:

```sql
  CREATE OR REPLACE PROCEDURE TEST_ME(
    PARAMETER1 IN VARCHAR2,
    RESULT OUT VARCHAR2
  )
  AS
  BEGIN
    RESULT := 'BEGIN';
    DBMS_OUTPUT.PUT_LINE(PARAMETER1);
    RESULT := 'END';
  END;
  /
```

아래처럼 호출하면:

```sql
DECLARE
  RESULT VARCHAR2(100);
BEGIN
  TEST_ME('hello!', RESULT);
  DBMS_OUTPUT.PUT_LINE(RESULT);
END;
COMMIT;
```

요로코롬 찍힌다:

```
hello!
END
```

끗.
