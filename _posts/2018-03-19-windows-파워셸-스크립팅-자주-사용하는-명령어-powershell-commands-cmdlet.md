---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '[Windows] 파워셸 스크립팅: 자주 사용하는 명령어 PowerShell Commands, Cmdlet'
categories:
  - windows
tags:
  - windows
  - powershell
  - terminal
  - shell
  - commands
  - cmdlet
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[Microsoft\] Cmdlet 개요](https://docs.microsoft.com/ko-kr/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7.2)

## 개요

악의축에서 갓갓으로 거듭나고 있는 마소의 파워셸 명령어 정리 글.

파워셸 명령어는 [Cmdlet](https://docs.microsoft.com/ko-kr/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7.2)이라고 한다. command-let으로 읽는다고 함. 이름은 동사-명사 형태로 만들고 단어의 처음은 대문자로 표기한다.

## 환경 변수

### 환경 변수 조회

```bash
# 모든 환경 변수 보기,
Get-ChildItem Env:
# 위와 같음
ls env:

# 환경 변수 중 'path' 출력,
Write-Output $env:path
# 위와 같음
echo $env:path
# 이렇게만 쳐도 됨 (암시적인 출력은 Write-Output이 처리함)
$env:path
```

### 로컬 환경 변수 추가/삭제

\* Windows Terminal은 새 탭이나 새 창을 열어도 환경 변수가 갱신되지 않는다. 완전히 끄고 재실행해야 함.

```bash
# 환경 변수 test 추가
$env:test = 1234

# 환경 변수 test2 추가
[Environment]::SetEnvironmentVariable("test2", "1234", "Process")

# 환경 변수 'test' 삭제
Remove-Item Env:\test
```

### 글로벌 환경 변수 추가/삭제

```bash
# 로그인한 사용자의 환경 변수로 'test' 추가
[Environment]::SetEnvironmentVariable("test", "1234", "User")

# 사용자 환경 변수 'test' 삭제
[Environment]::SetEnvironmentVariable("test", $null, "User")

# PATH 덧붙이기
[Environment]::SetEnvironmentVariable("PATH", "$env:PATH;원하는경로", "User")

# PATH 덧붙이기 #2
[Environment]::SetEnvironmentVariable("PATH", $env:PATH + ";원하는경로", "User")

# 시스템 환경 변수로 'test2' 추가. (관리자 권한 필요)
[Environment]::SetEnvironmentVariable("test2", "1234", "Machine")

# 시스템 환경 변수 'test2' 삭제. (관리자 권한 필요)
[Environment]::SetEnvironmentVariable("test2", $null, "Machine")
```

## 파워셸 명령어 기본 별칭

```
? -> Where-Object                 % -> ForEach-Object               ac -> Add-Content
cat -> Get-Content                cd -> Set-Location                chdir -> Set-Location
clc -> Clear-Content              clear -> Clear-Host               clhy -> Clear-History
cli -> Clear-Item                 clp -> Clear-ItemProperty         cls -> Clear-Host
clv -> Clear-Variable             cnsn -> Connect-PSSession         compare -> Compare-Object
copy -> Copy-Item                 cp -> Copy-Item                   cpi -> Copy-Item
cpp -> Copy-ItemProperty          cvpa -> Convert-Path              dbp -> Disable-PSBreakpoint
del -> Remove-Item                diff -> Compare-Object            dir -> Get-ChildItem
dnsn -> Disconnect-PSSession      ebp -> Enable-PSBreakpoint        echo -> Write-Output
epal -> Export-Alias              epcsv -> Export-Csv               erase -> Remove-Item
etsn -> Enter-PSSession           exsn -> Exit-PSSession            fc -> Format-Custom
fhx -> Format-Hex                 fl -> Format-List                 foreach -> ForEach-Object
ft -> Format-Table                fw -> Format-Wide                 gal -> Get-Alias
gbp -> Get-PSBreakpoint           gc -> Get-Content                 gcb -> Get-Clipboard
gci -> Get-ChildItem              gcm -> Get-Command                gcs -> Get-PSCallStack
gdr -> Get-PSDrive                gerr -> Get-Error                 ghy -> Get-History
gi -> Get-Item                    gin -> Get-ComputerInfo           gjb -> Get-Job
gl -> Get-Location                gm -> Get-Member                  gmo -> Get-Module
gp -> Get-ItemProperty            gps -> Get-Process                gpv -> Get-ItemPropertyValue
group -> Group-Object             gsn -> Get-PSSession              gsv -> Get-Service
gtz -> Get-TimeZone               gu -> Get-Unique                  gv -> Get-Variable
h -> Get-History                  history -> Get-History            icm -> Invoke-Command
iex -> Invoke-Expression          ihy -> Invoke-History             ii -> Invoke-Item
ipal -> Import-Alias              ipcsv -> Import-Csv               ipmo -> Import-Module
irm -> Invoke-RestMethod          iwr -> Invoke-WebRequest          kill -> Stop-Process
ls -> Get-ChildItem               man -> help                       md -> mkdir
measure -> Measure-Object         mi -> Move-Item                   mount -> New-PSDrive
move -> Move-Item                 mp -> Move-ItemProperty           mv -> Move-Item
nal -> New-Alias                  ndr -> New-PSDrive                ni -> New-Item
nmo -> New-Module                 nsn -> New-PSSession              nv -> New-Variable
ogv -> Out-GridView               oh -> Out-Host                    popd -> Pop-Location
ps -> Get-Process                 pushd -> Push-Location            pwd -> Get-Location
r -> Invoke-History               rbp -> Remove-PSBreakpoint        rcjb -> Receive-Job
rcsn -> Receive-PSSession         rd -> Remove-Item                 rdr -> Remove-PSDrive
ren -> Rename-Item                ri -> Remove-Item                 rjb -> Remove-Job
rm -> Remove-Item                 rmdir -> Remove-Item              rmo -> Remove-Module
rni -> Rename-Item                rnp -> Rename-ItemProperty        rp -> Remove-ItemProperty
rsn -> Remove-PSSession           rv -> Remove-Variable             rvpa -> Resolve-Path
sajb -> Start-Job                 sal -> Set-Alias                  saps -> Start-Process
sasv -> Start-Service             sbp -> Set-PSBreakpoint           scb -> Set-Clipboard
select -> Select-Object           set -> Set-Variable               shcm -> Show-Command
si -> Set-Item                    sl -> Set-Location                sleep -> Start-Sleep
sls -> Select-String              sort -> Sort-Object               sp -> Set-ItemProperty
spjb -> Stop-Job                  spps -> Stop-Process              spsv -> Stop-Service
start -> Start-Process            stz -> Set-TimeZone               sv -> Set-Variable
tee -> Tee-Object                 type -> Get-Content               where -> Where-Object
wjb -> Wait-Job                   write -> Write-Output
```

## Microsoft.Powershell.Core

### [Get-History](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-history?view=powershell-7.2)

명령어 실행 이력 보기. 기본 별칭 `history`

```bash
Get-History # 모든 명령어 이력 보기
Get-History 10 # 10번 째로 실행한 명령어 보기
Get-History -Count 10 # 명령어 이력을 마지막에서 거꾸로 10개만 보기
```

### [Invoke-History](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/invoke-history?view=powershell-7.2)

기본 별칭 `r`, `ihy`

```bash
Invoke-History # 마지막 명령어 실행
Invoke-History -Id 132 # 132번 명령어 실행
Invoke-History 132 # 위와 같음
```

### [Where-Object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.2)

프로퍼티를 기준으로 컬렉션에서 개체를 선택한다.

```bash
# name 프로퍼티가 'httpd.exe'인 개체 선택해서 출력
Get-ChildItem | Where-Object name -eq 'httpd.exe'
```

## Microsoft.PowerShell.Management

### [Get-Process](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-process?view=powershell-7.2)

프로세스 가져오기. 기본 별칭은 `ps`.

```bash
# PID가 2832 혹은 836인 프로세스 출력
Get-Process -Id 2832, 836

# 프로세스 이름 SoundSwitch인 프로세스 출력
Get-Process 'SoundSwitch'
```

#### parameters

- `-Id`: 하나 이상의 PID를 특정해서 필터링. 여러개일 땐 콤마`,`로 구분함
- `-Name`: 하나 이상의 프로세스 이름을 특정해서 필터링. 여러개일 땐 콤마`,`로 구분하며 파라미터명 `Name`은 생략할 수 있음.

### [Start-Process](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/start-process?view=powershell-7.2)

프로세스 시작. 기본 별칭은 `saps`.

```bash
Start-Process powershell –verb runAs # 관리자 권한으로 파워셸 실행
Start-Process explorer . # 현재 경로로 탐색기 실행(Start-Process는 생략 가능)
```

### [Stop-Process](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-process?view=powershell-7.2)

하나 이상의 프로세스를 중지하는 명령어. 기본 별칭은 `kill`.

```bash
# 이름이 SoundSwitch인 프로세스를 중지. (Get-Process랑 다르게 -Name 생략 불가)
Stop-Process -Name 'SoundSwitch'
```

### [Get-Service](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-service?view=powershell-7.2)

서비스 가져오기. 기본 별칭은 `gsv`.

#### parameters

- `-Name`: 파라미터의 값으로 서비스 이름을 특정한다. 와일드카드 사용 가능.

### [Stop-Service](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-service?view=powershell-7.2)

하나 이상의 서비스를 중지하는 명령어. 기본 별칭은 `spsv`.

```bash
Stop-Service -Name "sysmonlog"
```

### [Get-Content](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-content?view=powershell-7.2)

기본 별칭 `type`

```bash
Get-Content -Path nexus-2.14.5-02\logs\wrapper.log -Wait # 'tail -f'와 같음
```

### [Get-ChildItem](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.2)

기본 별칭 `ls`

```bash
# 현재 위치에서 모든 하위 파일과 폴더를 재귀 검색해서 출력하며 main.js로 필터링
Get-ChildItem -Recurse -Name | findstr main.js

# c:\dev\git 경로에서 README.md를 숨긴파일 포함하여 재귀검색하며 에러 났을 땐 그냥 넘어가고, 찾으면 경로와 파일명을 모두 출력
Get-ChildItem -Path C:\dev\git -Filter README.md -Recurse -Name -ErrorAction SilentlyContinue -Force
```

#### parameters

- `Recurse`: 재귀 검색
- `Name`: 현재 폴더 기준, 상대 경로와 파일명을 한 줄에 같이 표시한다.
- `Include`
- `Exclude`
- `ErrorAction`
- `Force`

### [Copy-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/copy-item?view=powershell-7.2)

기본 별칭 `copy`

```bash
Copy-Item .\dummy-for-copy.txt .\copy\clone.txt
```

### [Remove-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.2)

기본 별칭은 `del`, `erase`, `rd`, `ri`, `rm`, `rmdir` ~~많기도하네~~

```bash
Remove-Item .\copy\ -r -Force
```

#### parameters

- `-r`: 재귀삭제
- `-Force`: 확인 없이 삭제

### [Resolve-Path](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.management/resolve-path?view=powershell-7.2)

지정된 아이템의 전체 경로를 출력한다. 기본 별칭은 `rvpa`.

```bash
# 홈 디렉터리의 경로 출력
Resolve-Path ~

# 모든 하위 아이템(파일, 디렉터리)들의 경로 출력
Resolve-Path *

# package.json 파일의 경로 출력
Resolve-Path .\package.json

# C:\Program Files 하위 아이템들의 경로 출력
'C:\Program Files\*' | Resolve-Path

# 현재 경로부터 홈 디렉터리까지의 상대 경로 출력
Resolve-Path -Relative ~
```

## Microsoft.Powershell.Utility

### [Set-Variable](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.utility/set-variable?view=powershell-7.2)

현재 콘솔에 변수를 추가하거나 재할당한다. 유효범위가 세션이 아니라 콘솔이라서 새 탭이나 새 창의 터미널은 해당 변수를 공유하지 못함. 기본 별칭은 `set`, `sv`

```bash
Set-Variable test abcd
$test
# abcd

Set-Variable -Name qwer -Value 1234
$qwer
# 1234
```

### [Get-Variable](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.utility/get-variable?view=powershell-7.2)

변수 출력 명령어. 기본 별칭은 `gv`. 스코프를 지정하지 않으면 기본값은 로컬이다. 스코프에 대한 내용은 [여기에서 확인](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_scopes?view=powershell-7.2).

```bash
# 로컬 스코프의 모든 변수 출력
Get-Variable

# m으로 시작하는 모든 로컬 변수의 이름과 값 출력
Get-Variable m*

# m으로 시작하는 모든 로컬 변수의 값만 출력
Get-Variable -Name m* -ValueOnly

# m 혹은 p로 시작하는 로컬 변수의 이름과 값 출력
Get-Variable -Include m*, p*
```

### [Get-Host](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-host?view=powershell-7.2)

명령어를 입력하고 있는 호스트 프로그램(=파워셸)의 객체 정보를 출력함. 버전이나 언어 등이 나온다.

### [Out-String](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/out-string?view=powershell-7.2)

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

### [Get-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-alias?view=powershell-7.2)

설정된 별칭 목록을 출력한다. 기본 별칭 `gal`, `alias`

```bash
Get-Alias # 설정된 모든 별칭 출력
alias | Select-String -Pattern 'jb' -CaseSensitive # 소문자 jb가 포함된 모든 별칭 출력
gal -Definition Get-Alias # 설정된 별칭 중에 Get-Alias의 별칭 출력
```

### [Set-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.2)

신규 별칭 추가하거나 재할당한다. [New-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-alias?view=powershell-7.2)도 있는데 요건 재할당이 안되서 이미 있는 별칭이라면 에러가 발생한다.

```bash
# 탐색기의 별칭으로 ex 추가
Set-Alias ex explorer

# findstr의 별칭으로 grep 추가
Set-Alias grep findstr
# 위와 같음
Set-Alias -Name grep -Value findstr
```

이 명령을 터미널에서 직접 실행하면 현재 세션에서만 유효하게 된다. 앞으로의 모든 세션에 적용하려면 [파워셸 프로필](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2)에 추가한다. [관련 문서](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases).

일단 한 번 추가하면 프로파일의 파일 경로는 `$PROFILE` 변수로 찾을 수 있다:

```bash
code $PROFILE
```

만약 파라미터(=옵션)를 포함한 명령을 별칭으로 만들려면 프로필 파일에 아래처럼:

```bash
function Get-FilesIncludeHidden {
  Get-ChildItem -Force
}
Set-Alias -Name ll -Value Get-FilesIncludeHidden
```

함수를 정의하고 호출하도록 작성해야 한다. [관련문서](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.2#example-5--create-an-alias-for-a-command-with-parameters), [관련문서2](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7.2).

### [Write-Output](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7.2)

특정 객체를 파이프라인에 쓴다. 다른 Cmdlet로 파이프하거나 변수에 할당할 수 있다. 만약 Write-Output이 파이프라인의 마지막 명령인 경우 콘솔에 출력한다. 기본 별칭은 `echo`.

어떤 명령어를 사용할 때 발생하는 암시적인 출력은 Write-Output을 통한 출력이다.

```bash
# 파워셸 설치 경로 출력
Write-Output $PSHOME

# 비어있는 파일 생성. 'touch'와 같음
Write-Output $null >> dummy-for-commit.txt
```

### [Write-Host](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-host?view=powershell-7.2)

오직 콘솔 출력만을 위한 명령어. Write-Output과 달리 파이프라인에 보내지 않고 콘솔에 직접 쓴다. 따라서 다른 Cmdlet으로 파이프하거나 변수 할당은 할 수 없다. 기본 별칭은 없음.

```bash
PS> Write-Host '$abc:'$abc
$abc: 123
```

#### Write-Host와 Write-Output의 차이

Write-Output


### [Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.2)

웹 요청을 날리는 명령어. 리눅스의 `wget` 혹은 `curl`에 해당한다. 기본 별칭은 `iwr`.

```bash
# 단순 요청
Invoke-WebRequest -Uri "https://google.com"

# 헤더와 바디를 지정하는 방법
# 백틱(`)은 파워셸에서 줄바꿈을 의미함
Invoke-WebRequest -Method Get -Uri https://google.com/search `
  -Headers @{ 'Accept' = 'application/json'; 'X-My-Header' = 'Hello World' } `
  -Body @{ 'q' = 'Invoke-WebRequest+headers' }

# 파일 다운로드. OutFile 옵션으로 저장할 파일명을 지정함.
Invoke-WebRequest -Uri "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64" -OutFile "vscode.exe"
```
