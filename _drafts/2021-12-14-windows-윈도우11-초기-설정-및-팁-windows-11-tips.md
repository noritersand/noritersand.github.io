---
layout: post
date: 2021-12-14 14:00:01 +0900
title: '[Windows] 윈도우11 초기 설정 및 팁'
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

#### 참고한 문서

- [\[Microsoft\] Keyboard shortcuts in Windows](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)
- [\[Microsoft\] Windows의 바로 가기 키](https://support.microsoft.com/ko-kr/windows/windows의-바로-가기-키-dcc61a57-8ff0-cffe-9796-cb9706c75eec)

## 개요

CPU 구리면 설치도 못하는 윈도우11 초기 설정 및 팁 등을 정리함.

## 초기 설정

### 끌기 레이아웃 끄기

설정에서 `시스템 > 멀티태스킹 > 창 끌기 > 작업 표시줄 단추를 마우스로 가리킬 때 앱이 속한 끌기 레이아웃 표시` 체크 해제

이거 안하면 같은 앱을 여러 창으로 띄웠을 때 가끔 윈도우 키 + 번호키가 먹통되는 버그가 있음.

### 자판 배열 전화키 해제

다른 언어는 모르겠으나 한국어로 설치된 윈도우는 기본적으로 '입력 언어 전환'키와 '자판 배열 전환'키가 지정돼 있다. 문제는 얘네 때문에 특정 키 조합<kbd>ctrl + shift + 0</kbd>이 안먹는다.

설정에서 '고급 키보드 설정'으로 검색해서 진입한 뒤 '입력 언어 바로 가기 키' 클릭:

![](/images/let-me-press-ctrl-shift-0-bitch-1.png)

그리고 다음처럼:

![](/images/let-me-press-ctrl-shift-0-bitch-2.png)

'자판 배열 전환'을 할당 해제하면 됨. 여기까지만 해도 <kbd>ctrl + shift + 0</kbd>은 잘 작동한다. 찝찝하면 '입력 언어 전환'도 꺼버리자.

## 단축키

### 전역

- <kbd>win + a</kbd>: 빠른 설정 열기. 10에서 빠른 설정과 알림이 같이 있었는데 분리되었음.
- <kbd>win + n</kbd>: 알림과 달력 열기
- <kbd>win + q</kbd>, <kbd>win + s</kbd>: 시작 메뉴 검색
- <kbd>win + w</kbd>: 위젯 열기(웹 검색 같은데?)
- <kbd>win + h</kbd>: Microsoft 음성 명령 서비스
- <kbd>win + h</kbd>: 보이스 타이핑
- <kbd>alt + esc</kbd>: 창이 열렸던 순서대로 거꾸로 순환

### 창 크기/위치

- <kbd>win + z</kbd>: 스냅 레이아웃 열기
- <kbd>win + alt + 방향키</kbd>: 스냅 레이아웃 전환

## 작성자 저장용 작업표시줄 고정 설정

1. Terminal
2. Sublime Text
3. Firefox
4. Chrome
5. Sublime Merge
6. 가변: Discord, Slack
7. VSCODE
8. 가변: IDE#1
9. 가변: DBMS tool
0. 가변: IDE#2, 카톡, WorkFlowy
