---
layout: post
date: 2018-03-15 13:46:15 +0900
title: '[Windows] 윈도우 명령 프롬프트 명령어 aka CMD'
categories:
  - windows
tags:
  - os
  - cmd
  - shell
  - windows
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [윈도우 사용자라면? 꼭 알아야 할 명령 프롬프트 명령 14가지!](https://sergeswin.com/961)


## 개요

명령 프롬프트(윈도우 기본 셸인 Command prompt commands, 통칭 CMD)에서 자주 쓰이는 명령어 모음. 대부분 Powershell에서도 쓸 수 있는 명령어들이다.

여담으로 윈도우 설치 화면(별도의 설치 디스크로 부팅하면 설치할 디스크와 파티션을 설정하던 그 화면)에서는 `shift + f10`으로 cmd에 진입할 수 있음.


## 문법

### 파이프 `|`

둘 이상의 명령어를 연결


## 환경 변수

### [set](https://docs.microsoft.com/ko-kr/windows-server/administration/windows-commands/set_1)

환경 변수를 조회하거나 설정하는 명령어

```bash
set # 환경 변수 목록 조회
set a=1 # 환경 변수 a 추가
```


## 명령어

### find

특정 문자를 검색하는 명령어. 다른 명령어와 파이프로 연결해 출력을 필터링하는 용도로도 사용함.

```bash
dir | find "Videos" # "Videos"를 포함하는 라인만 출력
```

`find` 뒤에 오는 큰따옴표로 감싸진 문자열을 포함하는 라인만 출력한다. 윈도우답지 않게 대소문자를 구분함.

### findstr

`find`와 비슷하지만 정규식을 지원하는 명령어.

```bash
# '8081'로 필터링
netstat -nao | findstr '8081'
```

### copy

파일 복사

```bash
copy 원본_파일 복사될_파일
```

```bash
c:\dev\code-workspace>copy main.code-workspace main2.code-workspace
        1개 파일이 복사되었습니다.
```

### xcopy

파일과 폴더 트리를 복사하는 명령어.

```bash
xcopy SOURCE DESTINATION /s /e /y
```

#### Options

- `/S`: 비어 있지 않은 폴더와 하위 폴더를 복사.
- `/E`: 비어 있는 경우를 포함하여 폴더와 하위 폴더를 복사. /S /E 스위치와 같다.
- `/Y`: 기존 대상 파일을 덮어쓸지 여부를 묻지 않는다.

### netstat

```bash
# PID와 함께 모든 연결과 수신대기 포트를 숫자형식으로 출력하되
netstat -nao

# '8081'로 필터링
netstat -nao | findstr '8081'
```

### [nslookup](https://docs.microsoft.com/ko-kr/windows-server/administration/windows-commands/nslookup)

DNS 인프라 진단용 명령어.

```bash
# noritersand.github.io 도메인에 대한 DNS 정보 조회
nslookup noritersand.github.io

# dns.google.com 서버에서 icanhazip.com 검색
nslookup icanhazip.com dns.google.com
```

### tasklist

```bash
# 프로세스 목록을 출력하되 '18292'로 필터링
tasklist | findstr '18292'
```

### taskkill

```bash
# PID가 5888인 프로세스 중지
taskkill /f /pid 5888
```

### diskpart

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

### mklink

```bash
# 실제 경로는 \dest 폴더인 \slink 바로가기 링크 생성 (관리자 권한 필요)
mklink /d \slink \dest
```

### 정품 인증 관련

```bash
# 키 확인
wmic path softwarelicensingservice get OA3xOriginalProductKey

# 온라인으로 정품 인증
slmgr.vbs /ato

# 만료날짜 확인
slmgr.vbs -xpr

# 라이선스 등록 정보 확인
slmgr.vbs -dlv
```

### route

라우팅 테이블을 출력하거나 수정하는 명령어

```bash
# 도움말 보기
route /?

# 기본 출력
route PRINT
```

### ping

특정 IP 혹은 도메인으로 다른 네트워크 장비와의 연결 상태를 확인한다. ICMP(Internet Control Message Protocol)를 사용하기 때문에 상대 컴퓨터에서 ICMP를 차단하면 네트워크 연결이 정상이어도 `ping`은 작동하지 않는다.

```bash
# KT 서버에 중단없이 연결 테스트 + 현재 시각 출력
ping -t 168.126.63.1
```

### tree

현재 경로의 드라이브와 디렉터리 구조를 그래픽으로 출력한다.

```bash
tree /f /a
```

#### Options

- `/F`: 각 디렉터리의 파일 이름도 같이 출력한다.
- `/A`: 그래픽 문자(특수문자)를 텍스트 문자(아스키)로 대체한다.


## [certutil](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)

### 해시값 체크

```bash
certutil -hashfile 파일명 [해시알고리즘]
```

해시 알고리즘은 `MD2`, `MD4`, `MD5`, `SHA1`, `SHA256`, `SHA384`, `SHA512` 중에 하나여야 함. 생략했을 때의 기본값은 `SHA1`이다.

```bash
certutil -hashfile .\example.txt MD5
# SHA1의 .\example.txt 해시:
# f9c3df25f671c015100347d51cef76ee
# CertUtil: -hashfile 명령이 성공적으로 완료되었습니다.
```
