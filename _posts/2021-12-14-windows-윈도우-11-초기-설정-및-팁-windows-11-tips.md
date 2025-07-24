---
layout: post
date: 2021-12-14 14:00:01 +0900
title: '[Windows] 윈도우 11 초기 설정 및 팁'
categories:
  - windows
tags:
  - os
  - windows
  - environment
  - setup
  - shortcut
  - hotkey
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Keyboard shortcuts in Windows \| Microsoft Learn](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)
- [Windows의 바로 가기 키 \| Microsoft Learn](https://support.microsoft.com/ko-kr/windows/windows의-바로-가기-키-dcc61a57-8ff0-cffe-9796-cb9706c75eec)


## 개요

구형 CPU 쓰면 설치도 못하는 윈도우 11 초기 설정 및 팁 등 정리.


## 초기 설정

### 끌기 레이아웃 끄기

설정에서 `시스템 > 멀티태스킹 > 창 끌기 > 작업 표시줄 단추를 마우스로 가리킬 때 앱이 속한 끌기 레이아웃 표시` 체크 해제.

두 창을 맞춤상태로 했을 때의 그룹이 표시되는 기능인데, 켜두면 같은 앱을 여러 창으로 띄웠을 때 가끔 <kbd>win + 숫자</kbd>키가 먹통되는 버그가 있음.

최근 업데이트에서 '작업 표시줄 앱 위로 마우스를 가져갈 때, 작업 보기에서, Alt + Tab을 누를 때 내 스냅된 창 표시'로 바꼈고, 체크 상태라면 <kbd>win + tab</kbd>, <kbd>alt + tab</kbd>에서도 창 그룹이 표시된다.

### 자판 배열 전환키 해제

다른 언어는 모르겠으나 한국어로 설치된 윈도우는 기본적으로 '입력 언어 전환'키와 '자판 배열 전환'키가 지정돼 있는데, 문제는 얘네 때문에 <kbd>ctrl + shift + 0</kbd> 키 조합을 사용할 수 없고, <kbd>alt + shift</kbd>만 눌러도 입력 언어가 바뀐다.

설정에서 '고급 키보드 설정'으로 검색해서 진입한 뒤 '입력 언어 바로 가기 키' 클릭:

![](/images/let-me-press-ctrl-shift-0-bitch-1.png)

그리고 다음처럼:

![](/images/let-me-press-ctrl-shift-0-bitch-2.png)

'자판 배열 전환'을 할당 해제하면 됨. 여기까지만 해도 <kbd>ctrl + shift + 0</kbd>은 잘 작동한다. 찝찝하면 '입력 언어 전환'도 꺼버리자.

### alt + tab 목록에서 앱의 탭 제외하기

기본 설정 상으론 Edge 같은 앱의 탭 목록이 alt + tab에서도 보인다. 귀찮으니 `시스템 > 멀티태스킹 > 스냅하거나 Alt+Tab을 누를 때 앱의 탭 표시` 항목을 '탭 표시 안 함'으로 변경하자.

### 색 필터 단축키 끄기

설정에서 `접근성 > 색상 필터 > 색 필터용 바로 가기 키` 끄기


## 기타 팁

### 네트워크 장치 우선순위 지정하기

PC에 여러 네트워크 장치가 활성화되어 있을 때 네트워크 우선순위를 지정하는 방법이다.

GUI:

설정에서 `접근성 > 네트워크 및 인터넷 > 고급 네트워크 설정` 에서 네트워크 어댑터를 고르고, `추가 어댑터 옵션`에서 `TCP/IPv4`와 `TCP/IPv6`를 더블 클릭, `고급`으로 이동한 뒤 `자동 메트릭` 항목의 체크를 해제하고 `인터페이스 메트릭`의 값을 직접 지정한다.

ℹ️ 인터페이스 메트릭 값이 낮을 수록 우선순위가 높다.

CLI:

```bash
# 네트워크 인터페이스 목록
Get-NetIPInterface
```

`Get-NetIPInterface`로 원하는 네트워크 인터페이스의 `ifIndex` 값을 확인하고,

```bash
# 메트릭 지정하기
Set-NetIPInterface -InterfaceIndex <ifIndex> -InterfaceMetric 10
```

`Set-NetIPInterface`으로 지정하면 끗.

### 파일 탐색기에서 파일 목록에 포커싱하기

Windows 10에선 그냥 <kbd>space</kbd> 누르면 됐는데 이제는 <kbd>alt</kbd>, <kbd>esc</kbd>, <kbd>아래 방향키</kbd>를 순서대로 하나씩 눌러야 한다.

### 여러 파일을 한 번에 관리자 권한으로 실행하기

```bat
start "" "C:\전체경로\파일이름1"
start "" "C:\전체경로\파일이름2"
start "" "C:\전체경로\파일이름3"
```

이런식으로 드라이브부터 시작하는 전체경로를 작성한 뒤 `bat` 확장자로 저장한다. 그리고 이 bat 파일을 관리자 권한으로 실행하면 됨.

### 새 텍스트 파일을 만드는 가장 빠른 방법

파일 탐색기(File Explorer)에서 <kbd>alt + f</kbd>, <kbd>w</kbd>를 누르고 위 방향키(<kbd>↑</kbd>)로 *텍스트 문서*를 고른다.

### 파일 탐색기가 프리징 등으로 작동하지 않으면

*작업 관리자*<kbd>ctrl + shift + esc</kbd>에서 *Windows 탐색기* 혹은 *explorer*를 찾아 종료시킨다. 이렇게 하면 작업 표시줄이 사라지는데, *작업 관리자* 창에서 *새 작업 실행*을 누르고 `explorer`를 입력하면 다시 나타난다.

### 영타 간격이 이상할 때

```
show me the money
ｓｈｏｗ　ｍｅ　ｔｈｅ　ｍｏｎｅｙ
```

윈도우를 쓰다보면 어느 순간 이렇게 글자간 간격이 너무 넓어지는 경우가 있는데, '문자 너비'가 '반자'에서 '전자'로 변경됐기 때문이다.

되돌리려면 문자 너비 변경 단축키 <kbd>alt + =</kbd>를 누르거나, 트레이 근처에 있는 입력 언어 표시줄에서 변경하면 된다.

### 시스템 정보 보기

OS, 시스템(기기), BIOS, CPU, 메인보드, 메모리 등의 버전/모델명/제조사 등의 정보를 확인할 수 있음.

명령 실행 창(<kbd>win + r</kbd>)에서 `MSINFO32` 입력하면 됨.

### 코타나 강제 삭제

```bash
Get-AppxPackage *microsoft.549981C3F5F10* | Remove-AppxPackage
```


## 단축키

10에서 11로 넘어오며 추가/변경된 것만 정리함. [윈도우 10 게시글 링크](/windows/windows-윈도우-10-초기-설정-및-팁-windows-10-tips/)

### 전역

- <kbd>win + a</kbd>: *빠른 설정 열기* 10에서 빠른 설정과 알림이 같이 있었는데 분리되었음.
- <kbd>win + n</kbd>: *알림과 달력 열기*
- <kbd>win + q</kbd> <kbd>win + s</kbd>: *시작 메뉴 검색*
- <kbd>win + w</kbd>: *위젯 열기* 원래는 'Windows Ink 열기' 였음
- <kbd>win + h</kbd>: *보이스 타이핑* Microsoft 음성 명령 서비스다.
- <kbd>win + ctrl + v</kbd> *사운드 출력* 여러 출력 장치 중 하나를 선택하거나 음향 효과를 지정하는 창을 띄운다.
- <kbd>win + esc</kbd>: 돋보기가 켜졌을 때 누르면 꺼짐
- <kbd>win + space</kbd>: 입력기 변환
- <kbd>alt + space</kbd>: 원래는 창 시스템 메뉴 열기 기능이지만, PowerToys를 설치하면 PowerToys 실행 단축키로 바뀜.
- <kbd>alt + esc</kbd>: 창을 연 순서대로 거꾸로 순환. 안될 때도 있지만, 현재 활성화된 창을 즉시 최소화하는 기능으로 사용할 수 있음.
- <kbd>alt + shift + esc</kbd>: 창을 연 순서대로 순환

### 창 크기/위치

- <kbd>win + z</kbd>: ⭐스냅 레이아웃 열기. 활성화된 창의 위치를 조정할 때 사용한다. 최근 업데이트로 쓰기 편해졌음.
- <kbd>win + alt + 방향키</kbd>: 스냅 레이아웃 전환

### 파일 탐색기

- <kbd>alt + f</kbd> 혹은 <kbd>shift + 우클릭</kbd>: 파일 탐색기의 구 컨텍스트 메뉴를 활성화한다. Windows 10부터 추가된 새 컨텍스트 메뉴에서 '추가 옵션 표시'를 누르는 것과 같다.
- <kbd>Numpad Plus</kbd>: 사이드바에서 모든 하위 디렉터리 펼치기
- <kbd>Numpad minus</kbd>: 사이드바에서 모든 하위 디렉터리 접기
- <kbd>alt + p</kbd>: 미리보기 패널 열기


## 작성자 저장용 작업표시줄 고정 설정

- <kbd>win + 1</kbd>: Terminal
- <kbd>win + 2</kbd>: Sublime Text
- <kbd>win + 3</kbd>: Firefox
- <kbd>win + 4</kbd>: Chrome dev
- <kbd>win + 5</kbd>: Sublime Merge
- <kbd>win + 6</kbd>: Discord, Slack
- <kbd>win + 7</kbd>: WebStorm
- <kbd>win + 8</kbd>: Steam, VSCODE
- <kbd>win + 9</kbd>: 가변 #1 (ETC., IntelliJ)
- <kbd>win + 0</kbd>: 가변 #2 (MSOffice, Chrome)

가변 항목:

- 다른 브라우저
- Epic Games
- Bluestack
- IDE(IntelliJ, WebStorm, ...)
- MSOffice 문서

Autohotkey로 확장한 키 조합:

- <kbd>win + alt + 5</kbd>: Fork
- <kbd>win + alt + 6</kbd>: Discord
- <kbd>win + alt + 7</kbd>: Postman
- <kbd>win + alt + 0</kbd>: 가변 #3 (MSOffice)
- <kbd>win + - </kbd>: DBMS Tool(DataGrip, ...)
- <kbd>win + ctrl + - </kbd>: Notion
- <kbd>win + alt + - </kbd>: Obsidian
- <kbd>win + = </kbd>: Microsoft Edge
- <kbd>win + ctrl + = </kbd>: 야카오톡
- <kbd>win + backspace</kbd>: WorkFlowy
- <kbd>win + ctrl + backspace </kbd>: 아웃룩
- <kbd>win + alt + backspace </kbd>: 가변 #4

윈도우 기본 단축키 <kbd>alt + = </kbd> 때문인지 <kbd>win + alt + = </kbd> 조합은 작동하지 않음.

⚠️ <kbd>win + ctrl + 숫자</kbd> 조합은 기존 기능인 '마지막 활성 창으로 전환'을 자주 사용하기 때문에 다른 기능으로 재지정하면 안됨
