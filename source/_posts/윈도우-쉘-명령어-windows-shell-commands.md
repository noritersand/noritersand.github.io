---
title: 윈도우 쉘 명령어 windows shell commands
categories:
  - OS
  - windows
tags:
  - cmd
  - shell
  - todo
  - windows
date: 2018-03-15 13:46:15
---


## netstat
```bash
netstat -nao # PID와 함께 모든 연결과 수신대기 포트를 숫자형식으로 출력하되
netstat -nao | findstr '8081' # '8081'로 필터링
```

## tasklist
```bash
tasklist | findstr '18292' # 프로세스 목록을 출력하되 '18292'로 필터링
```

## taskkill
```bash
taskkill /f /pid 5888 # PID가 5888인 프로세스 중지
```

