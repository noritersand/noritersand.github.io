---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '윈도우10: 파워쉘 명령어 powershell commands'
categories:
  - os
  - windows
tags:
  - cmd
  - shell
  - powershell
  - windows
  - todo
---

* Kramdown table of contents
{:toc .toc}

## Get-Alias
기본 별칭: gal, alias
설정된 별칭 목록을 출력한다.
```bash
Get-Alias # 설정된 모든 별칭 출력
alias | Select-String -Pattern 'jb' -CaseSensitive # 소문자 jb가 포함된 모든 별칭 출력
gal -Definition Get-Alias # 설정된 별칭 중에 Get-Alias의 별칭 출력
```

## Start-Process
기본 별칭: saps
```bash
Start-Process powershell –verb runAs # 관리자 권한으로 파워쉘 실행
Start-Process explorer . # 현재 경로로 탐색기 실행(Start-Process는 생략 가능)
```

## Get-Content
기본 별칭: type
```bash
Get-Content -Path nexus-2.14.5-02\logs\wrapper.log -Wait # 'tail -f'와 같음
```

## Write-Output
기본 별칭: echo
```bash
Write-Output $null >> dummy-for-commit.txt # 'touch'와 같음
```

## Get-ChildItem
기본 별칭: ls

## 환경 변수

### 환경 변수 조회
```bash
Get-ChildItem Env: # 모든 환경 변수 보기, ls env:와 같음
Write-Output $env:path # 환경 변수 중 'path' 출력, echo $env:path와 같음. 사실 그냥 $env:path만 쳐도 된다
```

### 로컬 환경 변수 추가/삭제
```bash
$env:test = 1234 # 환경 변수 'test' 추가
[Environment]::SetEnvironmentVariable("test2", "1234", "Process") # 로컬 환경 변수 추가 두 번째 방법
Remove-Item Env:\test # 환경 변수 'test' 삭제
```

### 글로벌 환경 변수 추가/삭제
```bash
[Environment]::SetEnvironmentVariable("test", "1234", "User") # 로그인한 사용자의 환경 변수로 'test' 추가
[Environment]::SetEnvironmentVariable("test2", "1234", "Machine") # 시스템 환경 변수로 'test2' 추가, 이 명령은 관리자 권한 필요하다
[Environment]::SetEnvironmentVariable("test", $null, "User") # 사용자 환경 변수 'test' 삭제
[Environment]::SetEnvironmentVariable("test2", $null, "Machine") # 시스템 환경 변수 'test2' 삭제
```

## Invoke-WebRequest

웹 리퀘스트 날리는 명령어. `curl`과 거어어어어의 같음.

```bash
Invoke-WebRequest -Uri "https://google.com"
```
