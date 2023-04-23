---
layout: post
date: 2018-10-11 15:18:21 +0900
title: '[misc] Chocolatey'
categories:
  - misc
tags:
  - misc
  - chocolatey
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Chocolatey](https://chocolatey.org/)
- [Chocolatey: Packages](https://community.chocolatey.org/packages)

#### 버전 정보

- 1.2.1 시절에 작성한 글


## 개요

Winget에서 좀 더 발전된 패키지(혹은 애플리케이션) 관리 툴. 윈도우 전용이다. (맥은 Homebrew 있잖아. 윈도우도 WinGet 있는데?)


## 설치

[https://chocolatey.org/install](https://chocolatey.org/install)

파워셸(관리자 권한)에서 다음 줄 실행:

```bash
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```


## 명령어

[Chocolatey: commands](https://docs.chocolatey.org/en-us/choco/commands/)

### 패키지 설치와 제거

```bash
# winscp 설치
choco install winscp

# notepad++ 설치
choco install notepadplusplus

# 파폭과 크롬 설치
choco install firefox-dev googlechrome googlechrome.dev

# 이미 설치된 openjdk 패키지가 있어도 무시하고 강제 설치하되 프롬프트는 무조건 yes 입력
choco install openjdk --force -y

# mysql-cli 제거
choco uninstall mysql-cli
```

### 설치된 패키지 확인

```bash
# Chocolatey로 설치한 패키지 목록 출력
choco list --local

# Chocolatey로 설치한 것과 그렇지 않은것 모두 출력
choco list -li
```

### 패키지 버전 업그레이드

```bash
# 설치된 패키지 중 업그레이드 가능한 게 있는지 확인
choco outdated

# chocolatey를 최신 버전으로 업그레이드
choco upgrade chocolatey

# 모든 패키지를 최신 버전으로 업그레이드
choco upgrade all
```

### source(패키지 저장소) 보기/설정

```bash
# default source 보기
choco source

# source 추가
choco source add -n=bob -s="https://somewhere/out/there/api/v2/"
```

### 패키지 검색

```bash
choco find jdk # 'jdk'가 포함된 패키지 검색
choco search jdk # find의 alias
```


## [Update-SessionEnvironment](https://docs.chocolatey.org/en-us/create/functions/update-sessionenvironment)

Chocolatey에 포함된 기능으로 터미널 재시작 없이 환경 변수를 다시 불러올 수 있다.

명령어는 `RefreshEnv`... 지만, 관리자 권한 없이 실행한 터미널에선 그냥은 안되고 Chocolatey 설치 경로(`C:\ProgramData\chocolatey\bin`)를 path에 추가해야 됨.

