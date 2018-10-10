---
layout: post
date: 2018-03-15 13:46:15 +0900
title: '윈도우: 쉘 명령어 windows shell commands'
categories:
  - os
  - windows
tags:
  - cmd
  - shell
  - windows
---


## netstat

```bash
# PID와 함께 모든 연결과 수신대기 포트를 숫자형식으로 출력하되
netstat -nao

# '8081'로 필터링
netstat -nao | findstr '8081'
```

## tasklist

```bash
# 프로세스 목록을 출력하되 '18292'로 필터링
tasklist | findstr '18292'
```

## taskkill

```bash
# PID가 5888인 프로세스 중지
taskkill /f /pid 5888
```

## mklink

```bash
# 실제 경로는 \dest 디렉토리인 \slink 바로가기 링크 생성 (관리자 권한 필요)
mklink /d \slink \dest
```
