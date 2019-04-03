---
layout: post
date: 2018-03-15 13:46:15 +0900
title: '윈도우: 쉘 명령어 windows shell commands'
categories:
  - windows
tags:
  - os
  - cmd
  - shell
  - windows
---

## xcopy

파일과 디렉토리 트리를 복사하는 명령어.

```bash
xcopy SOURCE DESTINATION /s /e /y
```

#### options

- `/S`: 비어 있지 않은 디렉토리와 하위 디렉토리를 복사.
- `/E`: 비어 있는 경우를 포함하여 디렉토리와 하위 디렉토리를 복사. /S /E 스위치와 같다.
- `/Y`: 기존 대상 파일을 덮어쓸지 여부를 묻지 않는다.

## netstat

```bash
# PID와 함께 모든 연결과 수신대기 포트를 숫자형식으로 출력하되
netstat -nao

# '8081'로 필터링
netstat -nao | findstr '8081'
```

## certutil

https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil

### 해시값 체크

```bash
certutil -hashfile 파일명 [해시방식]
```

```bash
certutil -hashfile .\example.txt MD5
# SHA1의 .\example.txt 해시:
# f9c3df25f671c015100347d51cef76ee
# CertUtil: -hashfile 명령이 성공적으로 완료되었습니다.
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

## 정품 인증 관련

```bash
# 키 확인
wmic path softwarelicensingservice get OA3xOriginalProductKey

# 온라인으로 정품 인증
slmgr.vbs /ato

# 만료날짜 확인
slmgr.vbs -xpr

# 라이센스 등록 정보 확인
slmgr.vbs -dlv
```
