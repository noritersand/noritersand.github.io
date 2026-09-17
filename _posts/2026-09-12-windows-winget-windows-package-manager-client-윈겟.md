---
layout: post
date: 2026-09-12 16:03:04 +0900
title: '[Windows] WinGet (Windows Package Manager Client)'
categories:
  - windows
tags:
  - os
  - winget
  - package-manager
  - chocolatey
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- <https://github.com/microsoft/winget-cli>
- <https://github.com/microsoft/winget-pkgs>

#### 테스트 환경 정보

- WinGet 1.29.x


## 개요

Windows OS의 패키지 관리용 공식 CLI 툴. 리눅스의 `apt`, macOS의 `brew`와 비슷하다. Windows 10 이상이면 기본으로 설치되어 있어서 바로 사용할 수 있다.

2021년에 정식 출시했고, 초기에는 Chocolatey나 Scoop에 비해 등록된 패키지가 많지 않았지만, 지금(🗓️ 2026-09-12)은 패키지 생태계와 버전 관리가 크게 개선되어 패키지 수에서도 WinGet이 Chocolatey를 앞질렀다. 일반적인 Windows 개발 환경이라면 이제 Chocolatey나 Scoop 대신 WinGet으로 갈아타도 될 수준.


## 명령어

```bash
# 기본 도움말 보기
winget

# winget list의 도움말 보기
winget list -h

# KEYWORD로 패키지 검색
winget search KEYWORD

# KEYWORD로 NAME 부분일치 검색
winget search --name KEYWORD

# KEYWORD로 ID 부분일치 검색
winget search --id KEYWORD

# NAME이 KEYWORD와 정확히 일치하는 패키지만 보기(-e가 앞에 있어야 정상 작동함)
winget search -e --name KEYWORD

# PACKAGE 설치
winget install PACKAGE

# ID가 PACKAGE과 정확히 일치하는 패키지 설치
winget install -e --id PACKAGE

# PACKAGE 제거
winget uninstall PACKAGE

# PACKAGE 패키지의 상세정보 보기
winget show PACKAGE

# 설치된 패키지 목록을 출력. 버전 업그레이드가 가능한지도 표시됨
winget list

# 새 버전이 있는 패키지 보기
winget upgrade

# 특정 패키지만 버전 업그레이드
winget upgrade PACKAGE

# 모든 패키지의 버전 업그레이드
winget upgrade --all

# WinGet으로 설치한 모든 패키지 버전 업그레이드
winget upgrade --all --source winget
```

나머지:

- `winget source`: 패키지 데이터의 출처를 관리할 때 씀
- `winget hash`: 설치 관리자에 대한 SHA256 해시를 생성
- `winget validate`: 매니페스트 파일의 유효성 검사
- `winget settings`: WinGet 설정 파일을 열거나 관리자 설정을 관리한다.
- `winget features`: 실험적 기능의 상태 표시
- `winget export`: 설치된 패키지 목록 내보내기
- `winget import`: 패키지 목록이 담긴 파일을 읽어 지정된 패키지를 설치한다.
- `winget pin`: 특정 패키지의 업데이트를 제한하거나 버전을 고정한다.
- `winget configure`: 구성 파일을 읽어 Windows 환경을 원하는 상태로 설정한다.
- `winget download`: 앱을 설치하지 않고 설치 파일만 다운로드한다. 다운로드 경로는 사용자의 `Downloads` 폴더
- `winget repair`: 지정한 앱을 재설치 없이 복구한다.
- `winget dscv3`: PowerShell DSC v3에서 WinGet 리소스를 이용해 앱 설치 상태를 관리하는 하위 명령어. DSC(Desired State Configuration)는 Windows 시스템 설정을 코드로 관리할 수 있게 해주는 구성 관리 방식이다.
- `winget mcp`: WinGet MCP 서버의 실행 경로와 설정 정보를 출력한다.

WinGet은 Chocolatey와 다르게 공식 웹 카탈로그가 없다. <https://winget.run/>이 있는데, 공식이 아니라 서드파티라서 정확도가 좀...


## WinGet 패키지 등록

우선 WingetCreate를 설치한다:

```bash
winget install -e --id Microsoft.WingetCreate
```

그 다음 절차를 간단히 요약하면:

1. 앱 설치 파일을 준비한다. (`.exe`, `.msi` 등)
2. GitHub Releases 등에 버전별 설치 파일을 올린다.
3. `wingetcreate`로 manifest를 생성한다.
4. `winget validate`로 manifest를 검증하고 실제 설치도 테스트한다.
5. [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)에 pull request를 등록한다.
6. 자동 검증과 리뷰를 통과하면 merge된다.
7. 이후 사용자는 `winget install <PACKAGE>`로 설치할 수 있다.


끗.
