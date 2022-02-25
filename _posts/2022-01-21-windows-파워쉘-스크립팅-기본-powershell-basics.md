---
layout: post
date: 2022-01-21 13:43:45 +0900
title: '[Windows] 파워쉘 스크립팅: 기본 Powershell Basics'
categories:
  - windows
tags:
  - windows
  - powershell
  - terminal
  - shell
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[Microsoft\] PowerShell이란?](https://docs.microsoft.com/ko-kr/powershell/scripting/overview?view=powershell-7)
- [\[Microsoft\] PowerShell 설명서](https://aka.ms/powershell)
- [\[Microsoft\] Windows, Linux 및 macOS에 PowerShell 설치](https://docs.microsoft.com/ko-kr/powershell/scripting/install/installing-powershell?view=powershell-6#powershell-core)
- [\[Microsoft\] Microsoft.PowerShell.Core](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Core/?view=powershell-7.2)

## 개요

파워쉘에서 스크립트를 작성하고 사용하는 방법과 문법 등을 정리한 글.

## 스크립트 파일의 바로가기 만들기

아래는 Sound Switch 프로세스를 강제로 재시작하는 스크립트다:

```bash
# restart-soundswitch.ps1
Stop-Process -Name 'SoundSwitch'
Start-Process -FilePath 'C:\Program Files\SoundSwitch\SoundSwitch.exe'
```

문제는 파워쉘 스크립트 파일이 터미널 환경이 아니면 직접 실행할 수 없다는 것. 그래서 배치 파일을 추가로 만들고 거기서 파워쉘 스크립트를 실행한다:

```bash
# restart-soundswitch.bat
pwsh -executionpolicy remotesigned -File .\restart-soundswitch.ps1
```

이제 배치 파일의 바로가기를 만들어서 적절한 곳에 두면 끝. 혹시라도 `pwsh`가 안되면 `Powershell` 혹은 `Powershell.exe`로 바꾸면 됨.

## 문법

### [따옴표](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7.2) `""` `''`

리터럴 문자열을 표현할 때 큰 따옴표`""`와 작은 따옴표`''`는 대체로 동일한 의미로 쓰인다.  
다만 몇몇의 경우 차이가 있는데, 가령 다음 명령어 예시에서 변수 처리와 계산식은 홑따옴표를 사용할 때 무시된다:

```bash
$i = 5
"The value of $i is $i."
# The value of 5 is 5.

'The value of $i is $i.'
# The value of $i is $i.

"The value of $(2+3) is 5."
# The value of 5 is 5.

'The value of $(2+3) is 5.'
# The value of $(2+3) is 5.

"$env:LOCALAPPDATA\abcd"
# C:\Users\fixalot\AppData\Local\abcd

'$env:LOCALAPPDATA\abcd'
# $env:LOCALAPPDATA\abcd
```

## 변수선언과 사용

```bash
$abc = 1234

$abc
# 1234

gv abc
# Name      Value
# ----      -----
# abc       1234
```

관련 Cmdlet은 [여기](https://noritersand.github.io/windows/windows-파워쉘-스크립팅-자주-사용하는-명령어-powershell-commands-cmdlet/#heading-set-variable)서 확인.

## 연산자

[내부 링크](https://noritersand.github.io/windows/windows-파워쉘-스크립팅-연산자-powershell-operator/)

## 명령어 Cmdlet

[내부 링크](https://noritersand.github.io/windows/windows-파워쉘-스크립팅-자주-사용하는-명령어-powershell-commands-cmdlet/)

## [함수 Functions](https://docs.microsoft.com/ko-kr/powershell/scripting/learn/ps101/09-functions?view=powershell-7.2)

### 함수 정의

일단 생김새는 이렇다:

```bash
function Get-PSVersion {
  $PSVersionTable.PSVersion
}
```

함수 정의는 단순히 커맨드라인에서 함수 리터럴을 입력하면 된다. 하지만 이렇게 하면 현재 세션에만 유효한 함수가 되므로, [파워쉘 프로파일](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2)에 작성하거나 [스크립트 모듈](https://docs.microsoft.com/ko-kr/powershell/scripting/learn/ps101/10-script-modules?view=powershell-7.2)로 등록한다.

프로파일은 일단 한 번 추가하면 `$PROFILE` 변수에서 파일 경로를 찾을 수 있음:

```bash
Write-Output $PROFILE
# C:\Users\fixalot\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

함수의 이름은 [Cmdlet](https://docs.microsoft.com/ko-kr/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7.2)의 작명 규칙을 따르는게 좋다.

메모리에 함수가 로드됐는지는 'Function PSDrive'란 곳에서 볼 수 있다고 함:

```bash
# Function PSDrive에서 'Get-'으로 시작하고 'Version'으로 끝나는 함수 조회
Get-ChildItem -Path Function:\Get-*Version

# ABC 함수를 Function PSDrive에서 제거
Get-ChildItem -Path Function:\ABC | Remove-Item
```

### 파라미터

```bash
function Test-Fn {
  param (
    $Param1
  )
  echo $Param1
}
```

함수 블록 내에 `param(a, b, c, ...)`으로 파라미터를 정의한다. 단축 표기는 아래와 같다:

```bash
function Test-Fn($Param1) {
  echo $Param1
}

Test-Fn 123
# 123

Test-Fn -Param1 789
# 789
```

파라미터가 여러개일 경우 다음처럼 정의하고:

```bash
function Test-Fn($Param1, $Param2) {
  Write-Host '$Param1:'$Param1
  Write-Host '$Param2:'$Param2
}
```

호출할 때는 파라미터 구분을 `-파라미터이름`으로 지정하거나, 아니면 아예 생략한 뒤 순서에 맞춰 공백으로 구분한다:

```bash
Test-Fn abc def
# $Param1: abc
# $Param2: def

Test-Fn -Param2 abc -Param1 def
# $Param1: def
# $Param2: abc
```

하나의 파라미터는 쉼표`,`로 구분된 값 여러개를 전달할 수 있다. 이 때 해당 파라미터는 배열처럼 다룰 수 있다:

```bash
function Test-Fn($Param1) {
  Write-Host '$Param1:'$Param1
}

Test-Fn 1, 2, 3
# $Param1: 1 2 3

Test-Fn 1,2,3
# $Param1: 1 2 3

function Test-Fn2($Param1) {
  Write-Host '$Param1[0]:'$Param1[0]
  foreach ($ele in $Param1) {
    Write-Host '$ele:'$ele
  }
}

Test-Fn2 4, 5, 6
# $Param1[0]: 4
# $ele: 4
# $ele: 5
# $ele: 6
```

### 타입 제한

파라미터의 이름 앞에 대괄호`[]`로 감싸고 타입을 지정한다. 이렇게 하면 해당 타입이 아닌 값이 전달됐을 때 메시지와 함께 에러가 발생한다:

```bash
function Test-Fn3([string]$Param1) {}

Test-Fn3 123
Test-Fn3 a

function Test-Fn4([number]$Param1) {}

Test-Fn3 123
Test-Fn3 a
# InvalidOperation: Unable to find type [number].

function Test-Fn5([boolean]$Param1) {}

Test-Fn5 true
# Test-Fn5: Cannot process argument transformation on parameter 'Param1'. Cannot convert value "System.String" to type "System.Boolean". Boolean parameters accept only Boolean values and numbers, such as $True, $False, 1 or 0.

Test-Fn5 True
# Test-Fn5: Cannot process argument transformation on parameter 'Param1'. Cannot convert value "System.String" to type "System.Boolean". Boolean parameters accept only Boolean values and numbers, such as $True, $False, 1 or 0.

Test-Fn5 $True
Test-Fn5 $False
```

`string` 타입은 배열도 되는데 딱히 뭔가를 제한 기능은 음슴(왜냐면 길이 1짜리 배열도 있으니께):

```bash
function Test-Fn6([string[]]$Arr) {}

Test-Fn6 1
Test-Fn6 $True
Test-Fn6 a, b
```

### 필수 파라미터로 지정

`Parameter(Mandatory)` 혹은 `[Parameter(Mandatory=$true)]`로 해당 파라미터가 필수임을 지정한다:

```bash
# 단축 표기로 해도 되지만 가독성을 위해서 풀어서 씀
function Test-Fn7 {
  param (
    [Parameter(Mandatory)]
    [string]$Param1
  )
  Write-Host '$Param1:'$Param1
}

Test-Fn7
# cmdlet Test-Fn7 at command pipeline position 1
# Supply values for the following parameters:
# Param1:
# Test-Fn7: Cannot bind argument to parameter 'Param1' because it is an empty string.
```

필수 파라미터를 전달하지 않으면 이제라도 입력하라는 프롬프트가 뜬다. 이 때 <kbd>ctrl + c</kbd>하면 에러가 뙇.

`Parameter(Mandatory)`의 부수효과로 데이터 개수 제한이 있다. 위 예시에서 `$Param1`의 데이터 타입은 `string`인데 배열이 아니라서 딱 하나의 값만 허용한다:

```bash
Test-Fn7 -Param1 1
$Param1: 1

Test-Fn7 -Param1 1, 2, 3
# Test-Fn7: Cannot process argument transformation on parameter 'Param1'. Cannot convert value to type System.String.
```

### 기본값 지정

기본값은 `ValidateNotNullOrEmpty()`과 함께 등호`=`로 할당한다:

```bash
function Test-Fn9 {
  param (
    [ValidateNotNullOrEmpty()]
    [string]$Param1 = 'default value'
  )
  Write-Host '$Param1:'$Param1
}

Test-Fn9
# $Param1: default value
```

### 유효한 데이터 지정

특정 파라미터의 값으로 허용된 값만 받도록 제한하는 방법이다. `ValidateSet()`으로 지정한다:

```bash
function Test-Fn10 {
  param (
    [Parameter(Mandatory)]
    [ValidateSet('production', 'stage', 'test', 'vpn')]
    [string]$Server
  )
  Write-Host 'Chosen one:'$Server
}

Test-Fn10 -Server production
# Chosen one: production

Test-Fn10 -Server my-server
# Test-Fn10: Cannot validate argument on parameter 'Server'. The argument "my-server" does not belong to the set "production,stage,test,vpn" specified by the ValidateSet attribute. Supply an argument that is in the set and then try the command again.
```

이렇게 지정한 값은 탭 키를 눌러 자동완성이 된다:

![](/images/powershell-scripting-validateset3.gif)

아래 예시는 `ValidateSet()`와 해시테이블을 응용해서 ssh로 서버에 접속할 때 쓰는 함수다:

```bash
$hashtable = @{
  production = '1.2.3.4';
  stage = '2.3.4.5';
  test = '3.4.5.6';
  vpn = '4.5.6.7'
}

function Connect-RemoteServer {
  param (
    [Parameter(Mandatory)]
    [ValidateSet('production', 'stage', 'test', 'vpn')]
    [string]$Server
  )
  $target = $hashtable.$Server;
  echo $target
  ssh "ubuntu@$target"
}

Connect-RemoteServer -Server vpn
```

`@hashtable.keys`를 `ValidateSet()`에서 받아주면 좋겠는데 에러가 남:

```bash
function Connect-RemoteServer {
  param (
    [ValidateSet($hashtable.keys)]
    # ... 생략
  )
}

ParserError: C:\Users\fixalot\Documents\PowerShell\Microsoft.PowerShell_profile.ps1:24
Line |
  24 |      [ValidateSet($hashtable.keys)]
     |                   ~~~~~~~~~~~~~~~
     | Attribute argument must be a constant or a script block.
```
### CmdletBinding()

TODO

### 코멘트

TODO

### 파이프라인 입력 Pipeline Input

TODO

## [해시 테이블](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_hash_tables?view=powershell-7.2)

```bash
$hash = [ordered]@{ Number = 1; Shape = "Square"; Color = "Blue"}

$hash
# Name                           Value
# ----                           -----
# Number                         1
# Color                          Blue
# Shape                          Square

$hash.keys
# Number
# Shape
# Color

$hash.values
# 1
# Square
# Blue

$hash.Number
# 1
```

TODO
