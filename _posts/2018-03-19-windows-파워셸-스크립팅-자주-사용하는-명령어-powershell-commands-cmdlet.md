---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '[Windows] 파워셸 스크립팅: 자주 사용하는 명령어'
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

#### 참고 문서

- [Microsoft | Cmdlet 개요](https://docs.microsoft.com/ko-kr/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7.4)


## 개요

악의축에서 갓갓으로 거듭나고 있는 마소의 파워셸 명령어 정리 글.

파워셸 명령어는 *Cmdlet*이라 부른다. 'command-let'으로 읽는다고 함. 이름은 동사-명사 형태로 만들고 단어의 처음은 대문자로 표기한다.


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

[https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/?view=powershell-7.4](https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/?view=powershell-7.4)

### Get-Command

명령어(cmdlet), 함수, 별칭을 가져온다. 특정 명령어의 실제 실행 파일 위치 찾을 때도 쓰인다.

```bash
# 명령어 explorer의 명령어타입, 이름, 버전, 소스(경로) 출력
Get-Command explorer
```

### Get-History

명령어 실행 이력 보기. 기본 별칭 `history`

```bash
Get-History # 모든 명령어 이력 보기
Get-History 10 # 열 번째로 실행한 명령어 보기
Get-History -Count 10 # 명령어 이력을 마지막에서 거꾸로 10개만 보기
```

### Invoke-History

기본 별칭 `r`, `ihy`

```bash
Invoke-History # 마지막 명령어 실행
Invoke-History -Id 132 # 132번 명령어 실행
Invoke-History 132 # 위와 같음
```

### Where-Object

프로퍼티를 기준으로 컬렉션에서 개체를 선택한다.

```bash
# name 프로퍼티가 'httpd.exe'인 개체 선택해서 출력
Get-ChildItem | Where-Object name -eq 'httpd.exe'

# 확장자가 .jsp 이거나 .js 인 파일 출력
Get-ChildItem | Where-Object { $_.Extension -eq '.jsp' -or $_.Extension -eq '.js' }
```


## Microsoft.PowerShell.Management

[https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.management/?view=powershell-7.4](https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.management/?view=powershell-7.4)

### New-Item

파일이나 디렉터리, 심볼릭 링크 등을 생성한다.

```bash
# TARGET_PATH를 가리키는 심볼릭 링크 LINK 생성
New-Item -ItemType SymbolicLink -Path "LINK" -Target "TARGET_PATH"
```

#### parameters

- `-ItemType`: `File`, `Directory`, `SymbolicLink`, `Junction`, `HardLink` 중에 하나
- `-Path`: 생성할 심볼릭 링크의 이름
- `-Target`: 심볼릭 링크가 가리킬 대상 디렉터리

### Get-Process

프로세스 가져오기. 기본 별칭은 `ps`와 `gps`.

```bash
# PID가 2832 혹은 836인 프로세스 출력
Get-Process -Id 2832, 836

# 프로세스 이름 SoundSwitch인 프로세스 출력
Get-Process 'SoundSwitch'
```

#### parameters

- `-Id`: 하나 이상의 PID를 특정해서 필터링. 여러 개일 땐 콤마`,`로 구분함
- `-Name`: 하나 이상의 프로세스 이름을 특정해서 필터링. 여러 개일 땐 콤마`,`로 구분하며 파라미터명 `Name`은 생략할 수 있음.

### Start-Process

프로세스 시작. 기본 별칭은 `saps`.

```bash
Start-Process powershell –verb runAs # 관리자 권한으로 파워셸 실행
Start-Process explorer . # 현재 경로로 탐색기 실행(Start-Process는 생략 가능)
```

### Stop-Process

하나 이상의 프로세스를 중지하는 명령어. 기본 별칭은 `kill`.

```bash
# 이름이 SoundSwitch인 프로세스를 중지. (Get-Process랑 다르게 -Name 생략 불가)
Stop-Process -Name 'SoundSwitch'
```

### Get-Service

서비스 가져오기. 기본 별칭은 `gsv`.

#### parameters

- `-Name`: 파라미터의 값으로 서비스 이름을 특정한다. 와일드카드 사용 가능.

### Stop-Service

하나 이상의 서비스를 중지하는 명령어. 기본 별칭은 `spsv`.

```bash
Stop-Service -Name "sysmonlog"
```

### Get-Content

기본 별칭 `type`

```bash
Get-Content -Path nexus-2.14.5-02\logs\wrapper.log -Wait # 'tail -f'와 같음
```

### Get-ChildItem

지정된 위치의 아이템이나 하위 아이템의 객체 정보를 가져온다. 아이템은 파일이나 디렉터리 중 하나다. 기본 별칭은 `ls`. 

파라미터 없이 작동하지 않는 `Get-Item`과 다르게, `Get-ChildItem`은 명령어만 입력하면 현재 디렉터리 내에 있는 모든 파일과 디렉터리의 객체 정보를 가져온다.

```bash
# 현재 위치에서 모든 하위 파일과 폴더를 재귀 검색해서 출력하며 main.js로 필터링
Get-ChildItem -Recurse -Name | findstr main.js

# c:\dev\git 경로에서 README.md를 숨긴파일 포함하여 재귀 검색하며 에러 났을 땐 그냥 넘어가고, 찾으면 경로와 파일명을 모두 출력
Get-ChildItem -Path C:/dev/git -Filter README.md -Recurse -Name -ErrorAction SilentlyContinue -Force

# 현재 경로에서 재귀 검색 + 확장자가 js인 파일 찾기
Get-ChildItem -Recurse -Filter *.js

# 현재 경로에서 재귀 검색 + 확장자가 js인 파일 찾기 + FullName 프로퍼티를 콘솔 대신 temp2.md 파일에 쓰기
Get-ChildItem -Path . -Recurse -Filter *.js | Select-Object -Property FullName > temp.md

# 확장자가 js인 파일 찾기 + 파일의 전체 경로(=파일 객체의 FullName 프로퍼티)에서 "\target\"가 포함된 것은 제외
Get-ChildItem -Filter *.js | Where-Object { $_.FullName -notmatch '\\target\\' }

# 현재 경로에서 재귀 검색 + 확장자가 js인 파일 찾기 + 파일 내용 중 "axios"가 있는 라인 찾기 + 찾은 MatchInfo 객체에서 Path만 추출한 뒤 유일한 값만 출력
Get-ChildItem -Path . -Filter *.js -Recurse | Select-String -Pattern "axios" | Select-Object -Unique Path
```

#### parameters

- `Filter`: 특정 파일이나 폴더로 결과를 제한한다. 패턴은 와일드카드 패턴(Wildcard Patterns)이다.
- `Path`: 명령을 수행할 경로(명령의 시작 위치)
- `Recurse`: 재귀 검색 
- `Name`: 현재 폴더 기준, 상대 경로와 파일명을 한 줄에 같이 표시한다.
- `Include`: ?
- `Exclude`: ?
- `ErrorAction`: ?
- `Force`: ?

### Copy-Item

기본 별칭 `copy`

```bash
Copy-Item .\dummy-for-copy.txt .\copy\clone.txt
```

### Remove-Item

기본 별칭은 `del`, `erase`, `rd`, `ri`, `rm`, `rmdir` ~~많기도하네~~

```bash
Remove-Item .\copy\ -r -Force
```

#### parameters

- `-Force`: 확인 프롬프트 없이 삭제
- `-Recurse`: 재귀 삭제

### Get-Member

객체의 속성이나 메서드를 가져오는 명령어. 

파이프라인을 통해 전달된 객체가 어떤 타입 혹은 어떤 클래스의 인스턴스인지 확인하려면 단순히 아래처럼 입력하면 된다:

```bash
[명령어] | Get-Member
```

이러면 `TypeName` 섹션에서 객체의 타입을 확인할 수 있다. 

```bash
# System.IO.FileInfo, System.IO.DirectoryInfo 둘 중 하나
Get-ChildItem | Get-Member

# Microsoft.PowerShell.Commands.MatchInfo 타입
Get-ChildItem | Select-String foobar 
```

**TODO** 작동 방식 좀 더 분석

#### parameters

- `-MemberType`: 가져올 멤버의 타입을 지정한다.

### Resolve-Path

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

### Test-Connection

cmd의 `ping`과 같은 명령어. 지정한 컴퓨터에 ping(혹은 ICMP 에코 요청)을 보내고 결과를 출력한다.

```bash
# KT 서버에 중단없이 연결 테스트 + 현재 시각 출력
Test-Connection -Repeat -ComputerName 168.126.63.1 | Format-Table @{Name='TimeStamp';Expression={Get-Date}},Address,ProtocolAddress,ResponseTime

# 위 명령의 ping 버전
ping -t 168.126.63.1 | Foreach{"{0} - {1}" -f (Get-Date),$_}
```

**TODO** 사실 위 코드에서 `Test-Connection`은 주고 받은 바이트, 시간, TTL은 표시되지 않음. 찾아서 추가할 것


## Microsoft.Powershell.Utility

[https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.utility/?view=powershell-7.4](https://learn.microsoft.com/ko-kr/powershell/module/microsoft.powershell.utility/?view=powershell-7.4)

### Set-Variable

현재 콘솔에 변수를 추가하거나 재할당한다. 유효범위가 세션이 아니라 콘솔이라서 새 탭이나 새 창의 터미널은 해당 변수를 공유하지 못함. 기본 별칭은 `set`, `sv`

```bash
Set-Variable test abcd
$test
# abcd

Set-Variable -Name qwer -Value 1234
$qwer
# 1234
```

### Get-Variable

변수 출력 명령어. 기본 별칭은 `gv`. 스코프를 지정하지 않으면 기본값은 로컬이다. 스코프에 대한 내용은 [여기에서 확인](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_scopes?view=powershell-7.4).

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

### Get-Host

명령어를 입력하고 있는 호스트 프로그램(=파워셸)의 객체 정보를 출력함. 버전이나 언어 등이 나온다.

```bash
# 파워셸 버전 확인하기
Get-Host | Select-Object Version
```

### Out-String

오브젝트를 문자열로 출력한다. 이렇게 활용 가능:

```bash
# pwsh 프로세스 출력 결과를 result.txt 파일에 쓰기
Get-Process -Name pwsh | Out-String | Set-Content -Path ./result.txt
```

### Select-Object

객체나 객체의 프로퍼티를 선택하는 명령어. 보통은 다른 명령어와 파이프라인 입력으로 연결하여 사용한다.

```bash
# Get-ChildItem 결과의 처음부터 다섯 건만 출력
Get-ChildItem | Select-Object -First 5

# Get-ChildItem(=ls)으로 가져온 SOME_FILE의 객체 정보 중 디렉터리와 파일이름만 출력 
ls SOME_FILE | Select-Object -Property Directory, Name

# Get-Process에서 입력한 객체의 프로퍼티 중 ProcessName, Id, WS만 출력
Get-Process | Select-Object -Property ProcessName, Id, WS
```

#### parameters

- `-First`: 선택할 입력 객체의 수를 지정한다.
- `-Property`: 선택할 프로퍼티를 지정함
- `-Unique`: (보통은 파이프에 의해) 입력된 객체의 특정 프로퍼티를 기준으로 유일한 멤버만 선택

### Select-String

문자열이나 파일에서 특정 문자를 찾는 명령어. `grep`이나 `findstr`과 비슷하다.

```bash
# 'Hello'와 'HELLO' 출력 라인 중 'HELLO'만 필터링하되 대소문자를 구분하며 단순 일치하는지 판단
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch

# rogue.log 파일을 출력하되 대소문자 구분 없이 'exception'이 포함된 라인만 출력
Get-Content ./rogue.log | Select-String 'exception'

# 현재 경로의 모든 파일과 디렉터리를 Stream으로 내보내서 'httpd'가 포함된 라인 출력
Get-ChildItem | Out-String -Stream | Select-String 'httpd'

# detail.js에서 'qwer'가 몇 번 반복되는지 출력 
(Select-String -Path ./detail.js -Pattern qwer).Matches.Count
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

### Get-Alias

설정된 별칭 목록을 출력한다. 기본 별칭 `gal`, `alias`

```bash
Get-Alias # 설정된 모든 별칭 출력
alias | Select-String -Pattern 'jb' -CaseSensitive # 소문자 jb가 포함된 모든 별칭 출력
gal -Definition Get-Alias # 설정된 별칭 중에 Get-Alias의 별칭 출력
```

### Set-Alias

신규 별칭 추가하거나 재할당한다. [New-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-alias?view=powershell-7.4)도 있는데 요건 재할당이 안 되서 이미 있는 별칭이라면 에러가 발생한다.

```bash
# 탐색기의 별칭으로 ex 추가
Set-Alias ex explorer

# findstr의 별칭으로 grep 추가
Set-Alias grep findstr
# 위와 같음
Set-Alias -Name grep -Value findstr
```

이 명령을 터미널에서 직접 실행하면 현재 세션에서만 유효하게 된다. 앞으로의 모든 세션에 적용하려면 [파워셸 프로필](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.4)에 추가한다. [관련 문서](https://stackoverflow.com/questions/24914589/how-to-create-permanent-powershell-aliases).

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

함수를 정의하고 호출하도록 작성해야 한다. [관련문서](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.4#example-5--create-an-alias-for-a-command-with-parameters), [관련문서2](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7.4).

### Write-Output

특정 객체를 파이프라인에 쓴다. 다른 Cmdlet로 파이프하거나 변수에 할당할 수 있다. 만약 Write-Output이 파이프라인의 마지막 명령인 경우 콘솔에 출력한다. 기본 별칭은 `echo`.

어떤 명령어를 사용할 때 발생하는 암시적인 출력은 Write-Output을 통한 출력이다.

```bash
# 파워셸 설치 경로 출력
Write-Output $PSHOME

# 비어있는 파일 생성. 'touch'와 같음
Write-Output $null >> dummy-for-commit.txt
```

### Write-Host

오직 콘솔 출력만을 위한 명령어. Write-Output과 달리 파이프라인에 보내지 않고 콘솔에 직접 쓴다. 따라서 다른 Cmdlet으로 파이프하거나 변수 할당은 할 수 없다. 기본 별칭은 없음.

```bash
PS> Write-Host '$abc:'$abc
$abc: 123
```

#### Write-Host와 Write-Output의 차이

Write-Output


### Invoke-WebRequest

웹 요청을 날리는 명령어. 리눅스의 `wget` 혹은 `curl`에 해당한다. 기본 별칭은 `iwr`.

```bash
# 단순 요청
Invoke-WebRequest -Uri "https://google.com"

# 헤더와 바디를 지정하는 방법
# 백틱(`)은 파워셸에서 줄 바꿈을 의미함
Invoke-WebRequest -Method Get -Uri https://google.com/search `
  -Headers @{ 'Accept' = 'application/json'; 'X-My-Header' = 'Hello World' } `
  -Body @{ 'q' = 'Invoke-WebRequest+headers' }

# 파일 다운로드. OutFile 옵션으로 저장할 파일명을 지정함.
Invoke-WebRequest -Uri "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64" -OutFile "vscode.exe"
```

### Get-Filehash

지정한 파일의 해시값을 계산한다. 파라미터로 해시 알고리즘(SHA1, SHA256, SHA384, SHA512, MD5 등)을 선택할 수 있고 생략하면 SHA256을 사용한다.

```bash
Get-FileHash FILE_NAME

# MD5 알고리즘을 사용하면서 세로 목록 포맷으로 출력
Get-FileHash FILE_NAME -Algorithm MD5 | Format-list
```
