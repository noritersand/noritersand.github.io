---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '[Windows] 윈도우10 파워쉘 명령어 powershell commands'
categories:
  - windows
tags:
  - os
  - cmd
  - shell
  - powershell
  - windows
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Microsoft PowerShell 제품 설명서](https://aka.ms/powershell)
- [Microsoft Docs: PowerShell](https://docs.microsoft.com/ko-kr/powershell/scripting/overview?view=powershell-7)
- [크로스 플랫폼 PowerShell 설치](https://docs.microsoft.com/ko-kr/powershell/scripting/install/installing-powershell?view=powershell-6#powershell-core)

악의축에서 갓갓으로 거듭나고 있는 마소의 파워쉘 명령어 정리 글.

## [> , >> (리디렉션)](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_redirection?view=powershell-7)

```bash
명령어 > 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 지움
명령어 >> 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 추가
```

## | (파이프)

둘 이상의 명령어를 연결. 리눅스와 비슷하다.

## [Select-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7)

파워쉘판 `grep` 쯤에 위치하는 명령어.

```bash
# 'Hello'와 'HELLO' 출력 라인 중 'HELLO'만 필터링하되 대소문자를 구분하며 단순 일치하는지 판단
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch

# rogue.log 파일을 출력하되 대소문자 구분 없이 'exception'이 포함된 라인만 출력
Get-Content .\rogue.log | Select-String 'exception'
```

#### parameters

- `-CaseSensitive`: 대소문자를 구분하여 찾음. 생략하면 대소문자 무시.
- `-Pattern`: 이어지는 문자로 각 줄에서 찾을 텍스트를 지정함. 패턴 값은 정규식으로 취급됨.
- `-SimpleMatch`: 정규식 일치가 아니라 단순 일치로 필터링.
- TODO

#### Select-String AND, OR, NOT

```bash
# AND: 'c'와 '1'이 모두 포함된 라인만 출력
'xyz', 'abc', 'abc123' | Select-String 'c' | Select-String '1'

# OR: 'z' 혹은 '1'이 포함된 라인만 출력
'xyz', 'abc', 'abc123' | Select-String 'z|1'

# NOT: 'abc'가 없는 라인만 출력
'xyz', 'abc', 'abc123' | Select-String -NotMatch 'abc'
```

## Get-Alias

설정된 별칭 목록을 출력한다.
기본 별칭: `gal`, `alias`  

```bash
Get-Alias # 설정된 모든 별칭 출력
alias | Select-String -Pattern 'jb' -CaseSensitive # 소문자 jb가 포함된 모든 별칭 출력
gal -Definition Get-Alias # 설정된 별칭 중에 Get-Alias의 별칭 출력
```

## Start-Process

기본 별칭: `saps`

```bash
Start-Process powershell –verb runAs # 관리자 권한으로 파워쉘 실행
Start-Process explorer . # 현재 경로로 탐색기 실행(Start-Process는 생략 가능)
```

## Get-Content

기본 별칭: `type`

```bash
Get-Content -Path nexus-2.14.5-02\logs\wrapper.log -Wait # 'tail -f'와 같음
```

## Write-Output

기본 별칭: `echo`

```bash
Write-Output $null >> dummy-for-commit.txt # 'touch'와 같음
```

## Get-ChildItem

기본 별칭: `ls`

## Copy-Item

기본 별칭: `copy`

```bash
Copy-Item .\dummy-for-copy.txt .\copy\clone.txt
```

## Remove-Item

기본 별칭: `del`

```bash
Remove-Item .\copy\ -r -Force
```

## SSH

[마이크로소프트 공식 도움말: OpenSSH 키 관리](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_keymanagement)

### ssh-keygen

```bash
ssh-keygen
```

RSA 키 페어를 생성하는 명령어. 명령 실행 시 이름과 비밀번호를 묻는 프롬프트가 나타나며, 입력을 마치면 현재 경로에 공개키와 비공개키 하나씩 생성된다. 생성 단계에서 묻는 비밀번호는 2단계 인증용 비밀번호이며 입력하지 않아도 된다.

```bash
PS C:\Users\user\.ssh> ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\user/.ssh/id_rsa): noritersand-ssh-test
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in noritersand-ssh-test.
Your public key has been saved in noritersand-ssh-test.pub.
The key fingerprint is:
SHA256:ZOo+wm0BKnmG1njaqPdnDooGBhpH5OKUZjpN6ggizY4 user@DESKTOP-M8LV2E9
The key\'s randomart image is:
+---[RSA 2048]----+
| ..              |
| .o              |
|.*o     o        |
|OB. .  +         |
|X**. .. S        |
|%B+o ..          |
|E=*.....         |
| =.ooo*          |
|=....*o.         |
+----[SHA256]-----+

PS C:\Users\user\.ssh> ls

    디렉터리: C:\Users\user\.ssh

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----     2021-02-02  오전 10:36           1766 noritersand-ssh-test
-a----     2021-02-02  오전 10:36            403 noritersand-ssh-test.pub
```

### ssh-agent, ssh-add

비공개키를 관리하는 서비스와 명령어.

```bash
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\noritersand-ssh-test
```

추가한 비공개키는 Window 보안 컨텍스트에 저장된다고 한다. 마소는 이 작업 후 로컬 시스템에서 비공개키를 삭제하길 권장하고 있다.

#### options

- `-r`: 재귀삭제
- `-Force`: 확인 없이 삭제

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
