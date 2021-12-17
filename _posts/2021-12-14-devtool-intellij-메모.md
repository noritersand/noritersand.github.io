---
layout: post
date: 2021-12-14 18:23:22 +0900
title: '[devtool] IntelliJ 메모'
categories:
  - eclipse
tags:
  - devtool
  - intellij
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- ?

## 작성자 저장용 단축키 설정

- Open Recent Project: <kbd>ctrl + alt + shift + e</kbd> 최근 열었던 프로젝트 열기
- Find Acton...: <kbd>f1</kbd> 모든 명령 검색창인데 f1이 원래 도움말이었던거 지워버리고 요 키도 추가함.

## 기본 단축키

### 전역

- <kbd>shift, shift</kbd>: Search Everywhere 모든 것(?) 열기
- <kbd>ctrl, ctrl</kbd>: Run Anything 모든 것(?) run하기
- <kbd>ctrl + alt + s</kbd>: 설정
- <kbd>ctrl + shift + '</kbd>: 포커스가 있는 창 최대화(안되는 것도 있다)
- <kbd>ctrl + shift + a</kbd>: 실행 가능한 모든 명령 검색창
- <kbd>ctrl + shift + n</kbd>: Go to File 파일 열기
- <kbd>ctrl + e</kbd>: Recent Files 최근 파일 목록 보기
- <kbd>esc</kbd>: 파일 에디터로 포커스
- <kbd>(파일 에디터에서) shift + f4</kbd>: 현재 파일 새 창에서 보기
- <kbd>ctrl + f4</kbd>: 창 닫기
- <kbd>alt + home</kbd>: 파일 트리 탐색으로 포커스
- <kbd>ctrl + shift + f</kbd>: Find in Files 파일 내용으로 검색

### 파일 에디터

- <kbd>alt + f1</kbd>: 컨텍스트 메뉴 열기... 인가?
- <kbd>alt + f2</kbd>:
- <kbd>alt + f3</kbd>:
- <kbd>ctrl + alt + l</kbd>: 오토 포매팅
- <kbd>ctrl + shift + backspace</kbd>: 마지막 수정 지점으로 이동

## 한글 깨짐 문제

일단 발견한 인코딩 관련 설정은 요렇게 있다.

### \#1

Edit Custom VM Options로 이동(<kbd>ctrl + shift + a</kbd> 후 검색)한 뒤 아래 추가:

```
-Dfile.encoding=UTF-8
```

### \#2

Run/Debug Configurations 혹은 Services의 WAS 설정으로 이동해서 VM options에 아래 추가:

```
-Dfile.encoding=UTF-8
```

### \#3

Settings > Editor > File Encodings로 이동한 뒤:

- Project Encoding을 UTF-8로 변경
- Default encoding for properties files를 UTF-8, 그 옆에 Transparent native-to-ascii conversion 체크

### 그래서 되드나?

잘 모르겠는걸?

## 새 패키지 만들기

New > Directory가 아니고 Project Settings(<kbd>ctrl + alt + shift + s</kbd>)
