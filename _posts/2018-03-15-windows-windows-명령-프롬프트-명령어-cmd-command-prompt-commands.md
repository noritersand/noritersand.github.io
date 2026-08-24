---
layout: post
date: 2018-03-15 13:46:15 +0900
title: '[Windows] Windows 명령 프롬프트 명령어 CMD'
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

- [Windows 사용자라면? 꼭 알아야 할 명령 프롬프트 명령 14가지!](https://sergeswin.com/961)


## 개요

명령 프롬프트(Windows 기본 셸인 Command prompt commands, 통칭 CMD)에서 자주 쓰이는 명령어 모음. 

여담으로 Windows 설치 화면(별도의 설치 디스크로 부팅하면 설치할 디스크와 파티션을 설정하던 그 화면)에서는 `shift + f10`으로 cmd에 진입할 수 있음.

✂️ [이 블로그 내부 링크 \| Windows 노트](/windows/windows-windows-노트-윈도우-notes/)로 일부 이동함


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


## 셸 명령어

### copy

파일 복사

```bash
copy 원본_파일 복사될_파일
```

```bash
c:\dev\code-workspace>copy main.code-workspace main2.code-workspace
        1개 파일이 복사되었습니다.
```

### mklink

```bash
# 실제 경로는 \dest 폴더인 \slink 바로가기 링크 생성 (관리자 권한 필요)
mklink /d \slink \dest
```

### 정품 인증 관련 프로그램

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
