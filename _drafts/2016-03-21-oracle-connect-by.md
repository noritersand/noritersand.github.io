---
layout: post
date: 2016-03-21 21:05:00 +0900
title: 'Oracle: connect by'
categories:
  - dbms
  - oracle
tags:
  - oracle
  - sql
  - connect by
---

* Kramdown table of contents
{:toc .toc}

```sql
/* menuNumber와 연결된 상위 메뉴 조회 */
SELECT MENU_NO
FROM AD_MENU_INFO
START WITH MENU_NO = 1234
CONNECT BY PRIOR MENU_NO = UPR_MENU_NO
```
