---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '[Windows] 윈도우10 파워쉘 명령어 Powershell Commands'
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

## 문법

### [> , >> (리디렉션)](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_redirection?view=powershell-7)

```bash
명령어 > 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 지움
명령어 >> 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 추가
```

### [따옴표](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7.1)

리터럴 문자열을 표현할 때 큰 따옴표`""`와 작은 따옴표`''`는 대체로 동일한 의미로 쓰인다.  
다만 몇몇의 경우 차이가 있는데, 가령 다음 명령어 예시에서 변수 처리와 계산식은 홑따옴표를 사용할 때 무시된다:

```bash
PS> $i = 5
PS> "The value of $i is $i."
The value of 5 is 5.

PS> 'The value of $i is $i.'
The value of $i is $i.

PS> "The value of $(2+3) is 5."
The value of 5 is 5.

PS> 'The value of $(2+3) is 5.'
The value of $(2+3) is 5.
```

### | (파이프)

둘 이상의 명령어를 연결. 리눅스와 비슷하다.

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
Enter file in which to save the key (C:\Users\user/.ssh/id_rsa): noritersand-test
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in noritersand-test.
Your public key has been saved in noritersand-test.pub.
The key fingerprint is:
SHA256:uDDyhvysiAgiFBxuX47fZ+P3drfdtn3Ws8eFXTIkl/k user@noritersand-desktop
The key\'s randomart image is:
+---[RSA 2048]----+
| .             o |
|o .         . =  |
| =   .       + . |
|. o +  .      o E|
| ..oo.. S      =.|
|.. +.o..      . o|
|+ o o... +     .o|
|*. +    + .. . o%|
|+ ..o    .. o..*%|
+----[SHA256]-----+

PS C:\Users\user\.ssh> ls

    디렉터리: C:\Users\user\.ssh

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----     2021-02-02  오후 12:48           1766 noritersand-test
-a----     2021-02-02  오후 12:48            403 noritersand-test.pub
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

이렇게 추가한 비공개키는 Window 보안 컨텍스트에 저장된다고 한다. 마소는 이 작업 후 로컬 시스템에서 비공개키를 삭제하길 권장하고 있다.

## Microsoft.Powershell.Core

### [Get-History](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-history?view=powershell-7.1)

명령어 실행 이력 보기. 기본 별칭: `history`

```bash
Get-History # 모든 명령어 이력 보기
Get-History 10 # 10번 째로 실행한 명령어 보기
Get-History -Count 10 # 명령어 이력을 마지막에서 거꾸로 10개만 보기
```

### [Invoke-History](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/invoke-history?view=powershell-7.1)

기본 별칭: `r`, `ihy`

```bash
Invoke-History # 마지막 명령어 실행
Invoke-History -Id 132 # 132번 명령어 실행
Invoke-History 132 # 위와 같음
```

### [Where-Object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.1)

프로퍼티를 기준으로 컬렉션에서 개체를 선택한다.

```bash
# name 프로퍼티가 'httpd.exe'인 개체 선택해서 출력
Get-ChildItem | Where-Object name -eq 'httpd.exe'
```

## Microsoft.PowerShell.Management

### [Start-Process](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/start-process?view=powershell-7.1)

기본 별칭: `saps`

```bash
Start-Process powershell –verb runAs # 관리자 권한으로 파워쉘 실행
Start-Process explorer . # 현재 경로로 탐색기 실행(Start-Process는 생략 가능)
```

### [Get-Content](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-content?view=powershell-7.1)

기본 별칭: `type`

```bash
Get-Content -Path nexus-2.14.5-02\logs\wrapper.log -Wait # 'tail -f'와 같음
```

### [Get-ChildItem](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.1)

기본 별칭: `ls`

```bash
# 현재 위치에서 모든 하위 파일과 폴더를 재귀 검색해서 출력하며 main.js로 필터링
Get-ChildItem -Recurse -Name | findstr main.js

# c:\dev\git 경로에서 README.md를 숨긴파일 포함하여 재귀검색하며 에러 났을 땐 그냥 넘어가고, 찾으면 경로와 파일명을 모두 출력
Get-ChildItem -Path C:\dev\git -Filter README.md -Recurse -Name -ErrorAction SilentlyContinue -Force
```

#### options

- `Recurse`: 재귀 검색
- `Name`: 현재 폴더 기준, 상대 경로와 파일명을 한 줄에 같이 표시한다.
- `Include`
- `Exclude`
- `ErrorAction`
- `Force`

### [Copy-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/copy-item?view=powershell-7.1)

기본 별칭: `copy`

```bash
Copy-Item .\dummy-for-copy.txt .\copy\clone.txt
```

### [Remove-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.1)

기본 별칭: `del`

```bash
Remove-Item .\copy\ -r -Force
```

#### options

- `-r`: 재귀삭제
- `-Force`: 확인 없이 삭제

## Microsoft.Powershell.Utility

### [Get-Host](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-host?view=powershell-7.1)

명령어를 입력하고 있는 호스트 프로그램(=파워쉘)의 객체 정보를 출력함. 버전이나 언어 등이 나온다.

### [Out-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/out-string?view=powershell-7.1)

TODO

### [Select-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7)

문자열이나 파일에서 특정 문자를 찾는 명령어. `grep`이나 `findstr`과 비슷하다.

```bash
# 'Hello'와 'HELLO' 출력 라인 중 'HELLO'만 필터링하되 대소문자를 구분하며 단순 일치하는지 판단
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch

# rogue.log 파일을 출력하되 대소문자 구분 없이 'exception'이 포함된 라인만 출력
Get-Content .\rogue.log | Select-String 'exception'

# 현재 경로의 모든 파일과 디렉터리를 Stream으로 내보내서 'httpd'가 포함된 라인 출력
Get-ChildItem | Out-String -Stream | Select-String 'httpd'
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

### [Get-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-alias?view=powershell-7.1)

설정된 별칭 목록을 출력한다. 기본 별칭: `gal`, `alias`

```bash
Get-Alias # 설정된 모든 별칭 출력
alias | Select-String -Pattern 'jb' -CaseSensitive # 소문자 jb가 포함된 모든 별칭 출력
gal -Definition Get-Alias # 설정된 별칭 중에 Get-Alias의 별칭 출력
```

### [New-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-alias?view=powershell-7.1)

현재 세션에서만 유효한 신규 별칭 추가.

```bash
New-Alias grep findstr
```

앞으로의 모든 세션에 적용하려면 [파워쉘 프로필](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.1)에 추가한다. [관련 문서](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases).

난 어떻게 만든건지 `Documents\PowerShell\Microsoft.PowerShell_profile.ps1` 여기에 있음. 😒

### [Write-Output](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7.1)

기본 별칭: `echo`

```bash
Write-Output $PSHOME # 파워쉘 설치 경로 출력
Write-Output $null >> dummy-for-commit.txt # 비어있는 파일 생성. 'touch'와 같음
```

### [Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1)

웹 리퀘스트 날리는 명령어. `curl`과 거어어어어의 같음.

```bash
Invoke-WebRequest -Uri "https://google.com"
```

```bash
# grave(`)는 파워쉘에서 줄바꿈을 의미함
Invoke-WebRequest -Method Get -Uri https://google.com/search `
  -Headers @{ 'Accept' = 'application/json'; 'X-My-Header' = 'Hello World' } `
  -Body @{ 'q' = 'Invoke-WebRequest+headers' }
```
