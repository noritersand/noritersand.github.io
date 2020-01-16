---
layout: post
date: 2018-03-15 13:46:15 +0900
title: '[Windows] 윈도우 명령 프롬프트 명령어 windows command prompt commands'
categories:
  - windows
tags:
  - os
  - cmd
  - shell
  - windows
---

#### 관련 문서

- [윈도우 사용자라면? 꼭 알아야 할 명령 프롬프트 명령 14가지!](https://sergeswin.com/961)

명령 프롬프트(통칭 cmd. 윈도우 기본 쉘)에서 자주 쓰이는 명령어 모음. 대부분 Powershell에서도 쓸 수 있는 명령어들이다. **윈도우 설치 화면에서는 `shift + f10`으로 cmd에 진입할 수 있음.**

## xcopy

파일과 폴더 트리를 복사하는 명령어.

```bash
xcopy SOURCE DESTINATION /s /e /y
```

#### options

- `/S`: 비어 있지 않은 폴더와 하위 폴더를 복사.
- `/E`: 비어 있는 경우를 포함하여 폴더와 하위 폴더를 복사. /S /E 스위치와 같다.
- `/Y`: 기존 대상 파일을 덮어쓸지 여부를 묻지 않는다.

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

## diskpart

윈도우 전통의 디스크 관리 명령어. 디스크, 파티션, 볼륨 등을 확인하고 지정/변경할 수 있다.

```
Microsoft Windows [Version 10.0.18363.535]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Users\noriter>diskpart

Microsoft DiskPart 버전 10.0.18362.1

Copyright (C) Microsoft Corporation.
컴퓨터: NORITERSAND-LAPTOP

DISKPART> list volume

  볼륨 ###  Ltr  레이블      Fs    형식       크기     상태          정보
  --------  ---  ----------  ----  ---------  -------  ------------  --------
  Volume 0         시스템 예약       NTFS   파티션          549 MB  정상         시스템
  Volume 1     C                NTFS   파티션          237 GB  정상         부팅
  Volume 2     D                NTFS   파티션          931 GB  정상

DISKPART> list disk

  디스크 ###  상태           크기     사용 가능     Dyn  Gpt
  ----------  -------------  -------  ------------  ---  ---
  디스크 0    온라인        238 GB       1024 KB
  디스크 1    온라인        931 GB           0 B

DISKPART> select disk 0

0 디스크가 선택한 디스크입니다.

DISKPART> list partition

  파티션 ###  종류              크기     오프셋
  ----------  ----------------  -------  -------
  파티션 1    주                  549 MB  1024 KB
  파티션 2    주                  237 GB   550 MB
  파티션 3    복구                 562 MB   237 GB
```

## [certutil](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)

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

## mklink

```bash
# 실제 경로는 \dest 폴더인 \slink 바로가기 링크 생성 (관리자 권한 필요)
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
