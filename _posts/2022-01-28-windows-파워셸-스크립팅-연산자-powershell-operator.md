---
layout: post
date: 2022-01-28 23:43:45 +0900
title: '[Windows] 파워셸 스크립팅: 연산자'
categories:
  - windows
tags:
  - windows
  - powershell
  - terminal
  - shell
  - Operator
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [\[Microsoft\] about_Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.2)
- [\[Microsoft\] about_Operators 우리말 버전](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.2)


## 개요

파워셸 스크립팅의 연산자 부분만 정리한 글.


## 리디렉션 연산자 `>` `>>`

```bash
명령어 > 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 지움
명령어 >> 파일명  # 파일이 없으면 생성하고, 있으면 기존내용을 추가
```


## [특수 연산자 Special Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.2#special-operators)

### Grouping operator `( )`

### Subexpression operator `$( )`

괄호 안 표현식의 실행 결과를 반환한다. 명령의 결과를 문자열 혹은 또다른 명령에 포함시킬 때 사용한다:

```bash
# yarn global bin: Yarn의 글로벌 경로를 반환하는 명령어
# 경로를 받아온 다음 해당 경로로 전환
cd $(yarn global bin)
```

### Array subexpression operator `@( )`

TODO

### 해시 테이블 리터럴 구문 Hash table literal syntax `@{}`

TODO

### 파이프라인 연산자 `|`

둘 이상의 명령어를 연결. 리눅스와 비슷하다. '앞에 오는 명령의 출력을 뒤에 오는 명령으로 보낸다(파이프한다)'라고 표현한다.

예를 들어 유틸리티 모듈에는 `Format-List`라는 리스트를 세로 목록으로 출력하는 명령이 있는데, 파이프를 활용하면:

```bash
PS> Get-FileHash .\upload.me | Format-list

Algorithm : SHA256
Hash      : 90B56139615DA8FE23201FD4C5FFE6E40EB16A8D544387B8056D2E1CF8D4AFF9
Path      : C:\dev\upload.me
```

이렇게 된다.

### 호출 연산자 Call Operator `&`

호출 연산자는 문자열을 실행할 때 사용한다. 

자바스크립트의 `eval()`과 비슷하다. 예를 들어 서브라임의 실행 파일을 전체 경로로 실행하려고 할 때, 공백을 포함한 전체 경로를 지정하려면 다음처럼 해야하는데:

```bash
'C:\Program Files\Sublime Text\subl.exe'
# C:\Program Files\Sublime Text\subl.exe 출력됨
```

이러면 파일을 실행하는게 아니라 문자열을 출력해 버린다. 이럴 땐 호출 연산자를 앞에 붙여서 문자열을 스크립트로 실행하도록 하면 된다:

```bash
& 'C:\Program Files\Sublime Text\subl.exe'
# 서브라임을 실행함
```

그런데 호출 연산자는 문자열을 **구문 분석** 하지 않는다고 한다. 그래서 명령어의 파라미터를 사용할 수 없다:

```bash
$c = "Get-Service -Name Spooler"

& $c
# &: The term 'Get-Service -Name Spooler' is not recognized as a name of a cmdlet, function, script file, or executable program.
# Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
```

이럴 땐 호출 연산자 대신 [Invoke-Expression](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.2)을 사용하라고 함:

```bash
Invoke-Expression $c
# Status   Name               DisplayName
# ------   ----               -----------
# Running  Spooler            Print Spooler
```

### 백그라운드 연산자 Background operator `&`

명령을 백그라운드에서 실행한다. 호출 연산자와 다르게 앰퍼샌드가 명령 마지막에 위치한다.

```bash
# https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.3#background-operator-

Get-Process -Name pwsh &
# 위와 같음
Start-Job -ScriptBlock {Get-Process -Name pwsh}
```

### 캐스트 연산자 Cast operator `[ ]`

TODO

### 쉼표 연산자 Comma operator `,`

TODO

### 점 소싱 연산자 Dot sourcing operator `.`

```bash
. $PROFILE
```

특정 스크립트를 읽어서 현재 범위에 변수, 함수등을 재정의한다. 리눅스의 `source`와 비슷. `.` 뒤에 공백이 한 칸 있어야 함.
