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

- [https://www.jetbrains.com/help/idea/getting-started.html](https://www.jetbrains.com/help/idea/getting-started.html)

#### 버전 정보

- IntelliJ IDEA 2021.3

## 개요

인텔리제이 관련 단축키와 설정 등의 간단 메모 글.

자바 개발자들 사이에선 인텔리제이 한 번 써보면 다시는 이클립스로 못돌아간다는 전설이 있다고 하더라. ~~뻥임~~

## 초기 설정

TODO

## 작성자 저장용 단축키 설정

- Open Recent Project: <kbd>ctrl + alt + shift + e</kbd> 최근 열었던 프로젝트 열기
- Find Acton...: <kbd>f1</kbd> 모든 명령 검색창인데 f1이 원래 도움말이었던거 지워버리고 요 키도 추가함.
- Undo: <kbd>ctrl + z</kbd>: 되돌리기. 다른 키는 다 지움.
- Redo: <kbd>ctrl + y</kbd>: 다시 되돌리기. 다른 키는 다 지움.
- Delete Line: <kbd>ctrl + shift + d</kbd> 라인 삭제. 기존 키 매핑은 삭제.
- Clone Caret Above: <kbd>ctrl + alt + up</kbd> 위로 멀티 캐럿 생성.  다른 키는 지움.
- Clone Caret Below: <kbd>ctrl + alt + down</kbd> 아래로 멀티 캐럿 생성.  다른 키는 지움.
- Duplicate Line or Selection: <kbd>ctrl + shift + k</kbd> 중복 라인 생성. 기존 키 매핑은 삭제.

## Ultimate와 Community 버전 간 차이

지원하는 언어, 프레임웍에서 차이가 많이 난다. 몇 개만 꼽자면 Community 버전은 Spring, Java EE, JavaScript, TypeScript, Node.js, PHP, SQL 등을 미지원. 사실 순수 자바 프로젝트는 그냥 커뮤니티 버전 써도 됨. (JS 같은 건 VSCODE로 한다 치고)

자세한 내용은 [여기](https://www.jetbrains.com/products/compare/?product=idea&product=idea-ce)에.

## TODO 톰캣 퍼블리싱 폴더는 어디일까

이클립스의 고것과 같은 경로를 못찾겠다. 설마 target을 직접 보는건지.

## 빌드나 런타임 에러가 발생하면

Project Structure<kbd>ctrl + alt + shift + s</kbd>에서 Modules 항목 설정은 이렇게 돼있는지 우선 확인:

![](/images/intellij-project-settings-2.png)

## 한글 깨짐 문제

일단 발견한 인코딩 관련 설정은 요렇게 있다.

### \#1

Edit Custom VM Options로 이동(<kbd>ctrl + shift + a</kbd> 후 검색)한 뒤 `-Dfile.encoding=UTF-8` 추가

### \#2

Run/Debug Configurations 혹은 Services의 WAS 설정으로 이동해서 VM options에 `-Dfile.encoding=UTF-8` 추가

### \#3

Settings > Editor > File Encodings로 이동한 뒤:

- Project Encoding을 UTF-8로 변경
- Default encoding for properties files를 UTF-8, 그 옆에 Transparent native-to-ascii conversion 체크

### 그래서 되드나?

잘 모르겠는걸?

## 추천 플러그인

- MoveTab: 단축키로 탭 이동하고 싶으면 설치. 단축키는 Move Tab Left/Right 찾아서 <kbd>shift + ctrl + alt + pageup/pagedown</kbd>으로 변경

## 새 패키지 만들기

New > Directory가 아니고 Project Settings<kbd>ctrl + alt + shift + s</kbd>

## File and Code Templates

TODO

작성 예시:

```java
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ${NAME} {
    private static final Logger logger = LoggerFactory.getLogger(${NAME}.class);
}

```

## 자동 완성

### Code Completion

TODO

### Postfix Completion

규칙대로 입력한 뒤 'Code Completion'을 발동하면 미리 작성한 코드가 자동으로 입력된다.

TODO

### File and Code Templates

`Settings` > `Editor` > `File and Code Templates`

### Live Templates

이클립스에서 특정 키워드 후의 자동 완성과 같은 기능이다.

`Settings` > `Editor` > `Live Templates`으로 이동해서 그룹 선택 후 우측의 십자 모양 아이콘 클릭한 다음 요딴식으로 작성한다:

```java
Logger logger = LoggerFactory.getLogger($className$.class);
```

`$className$`은 미리 정해져있는 값이 아니고 `Edit variables`를 눌러서 사용자가 별도로 지정하는 변수다.

마지막으로 `applicable contexts`를 지정하면 끝. (위 예시의 경우 멤버 변수 선언이기 때문에 Java - declaration으로 함)

사전 정의 변수는 두 개로 `$END$`와 `$SELECTION$`이다. 도움말은 [여기](https://www.jetbrains.com/help/idea/template-variables.html)를 보면 됨.

몇 가지 더 예를 들면:

```java
// Template Text // 변수 -> Expression, 적용 범위
logger.info("{}", $aaa$); // $aaa$ -> completeSmart(), expression
logger.error($var$.getMessage(), $var$); // $var$ -> variableOfType("Exception"), expression
private static final Logger logger = LoggerFactory.getLogger($className$.class); // $className$ -> className(), declaration
```

#### 사전 정의 변수 Predefined template variables﻿

- `$END$`: 라이브 템플릿 작동 후 커서의 위치를 지정
- `$SELECTION$`: 특정 코드를 선택(드래그)한 뒤 Surround With...<kbd>ctrl + alt + t</kbd>로 라이브 템플릿을 선택하면 지정한 위치에 선택했던 코드가 자동으로 입력됨

## 기본 단축키 메모

### 전역

- <kbd>esc</kbd>: 파일 에디터로 포커스
- <kbd>alt + insert</kbd>: 새로 추가. 상황에 따라 구성요소/파일/rule 등을 추가하는 메뉴를 보여줌.
- <kbd>shift, shift</kbd>: Search Everywhere 모든 것(?) 열기
- <kbd>ctrl, ctrl</kbd>: Run Anything 모든 것(?) run하기
- <kbd>ctrl + alt + s</kbd>: Settings... 인텔리제이 설정
- <kbd>ctrl + alt + shift + s</kbd>: Project Structure... 현재 프로젝트 전용 설정
- <kbd>ctrl + shift + '</kbd>: 포커스가 있는 창 최대화(안되는 것도 있다)
- <kbd>ctrl + shift + a</kbd>: 실행 가능한 모든 명령 검색창
- <kbd>ctrl + shift + n</kbd>: Go to File 파일 열기
- <kbd>ctrl + e</kbd>: Recent Files 최근 파일 목록 보기
- <kbd>ctrl + shift + e</kbd>: Recently Changed Files 최근 변경된 파일 목록 보기
- <kbd>ctrl + f4</kbd>: 창 닫기
- <kbd>alt + home</kbd>: 파일 트리 탐색으로 포커스
- <kbd>ctrl + shift + f</kbd>: Find in Files 파일 내용으로 검색

### 북마크

- <kbd>ctrl + shift + 1 부터 0까지</kbd>: 현재 파일과 라인을 북마크로 지정하거나 해제
- <kbd>ctrl + 1 부터 0까지</kbd>: 지정한 북마크로 이동
- <kbd>ctrl + f11</kbd>: 북마크 등록 창 열기
- <kbd>ctrl + shift + f11</kbd>: 북마크 이동 창 열기

### 윈도우 관련

- <kbd>alt + 1</kbd>: Project 윈도우로 포커싱
- <kbd>alt + 2</kbd>: Bookmarks 윈도우로 포커싱
- <kbd>alt + 3</kbd>: Find 윈도우로 포커싱
- <kbd>alt + 4</kbd>: Run 윈도우로 포커싱
- <kbd>alt + 5</kbd>: Debug 윈도우로 포커싱
- <kbd>alt + 6</kbd>: Problems 윈도우로 포커싱
- <kbd>alt + 7</kbd>: Structure 윈도우로 포커싱
- <kbd>alt + 8</kbd>: Services 윈도우로 포커싱
- <kbd>alt + 9</kbd>: Git 윈도우로 포커싱
- <kbd>alt + 0</kbd>: Commit 윈도우로 포커싱
- <kbd>ctrl + h</kbd>: Hierarchy 윈도우로 포커싱
- <kbd>shift + esc</kbd>: 현재 윈도우 최소화
- <kbd>ctrl + shift + '</kbd>: 현재 윈도우 최대화, 다시 누르면 원래 크기로

### 파일 에디터

- <kbd>ctrl + q</kbd>: Quick Documentation 툴팁창으로 자바독 보기
- <kbd>ctrl + b</kbd>: 정의된 파일이나 선언부로 이동, 이미 선언부일 땐 참조하는 코드 미리보기
- <kbd>ctrl + alt + b</kbd>: (인터페이스의) 구현부가 따로 있으면 그 쪽으로, 아니면 선언부로 이동
- <kbd>alt + enter</kbd>: Show Context Actions 파일 에디터에서 발동하면 상황에 맞는 메뉴 보여줌. 대부분 리펙토링 관련.
- <kbd>alt + f1</kbd>: Select in... 어느 윈도우에서 현재 파일(혹은 포커스가 있는 요소)을 보여줄 지 선택하는 창이 열림. <kbd>alt + f1, 1</kbd> 누르면 프로젝트 윈도우에서 현재 파일이 보이는 식.
- <kbd>alt + f2</kbd>:
- <kbd>alt + f3</kbd>:
- <kbd>alt + f7</kbd>: Find Usages 포커스된 대상이 어디서 쓰이고 있는지 프로젝트 전체 검색
- <kbd>ctrl + f7</kbd>: Find Usages 포커스된 대상이 어디서 쓰이고 있는지 현재 파일 내 검색
- <kbd>ctrl + f1</kbd>: Error Description 에러 툴팁 보기
- <kbd>ctrl + f12</kbd>: File Structure  eclipse의 빠른 아웃라인 보기 기능과 같음.
- <kbd>ctrl + alt + l</kbd>: 오토 포매팅
- <kbd>ctrl + shift + backspace</kbd>: 마지막 수정 지점으로 이동
- <kbd>shift + f4</kbd>: 현재 파일 새 창에서 보기
- <kbd>f2</kbd> or <kbd>shift + f2</kbd>: Highlighted Error 다음/이전 에러 지점으로 이동
- <kbd>ctrl + alt + 방향키좌우</kbd>: 이전/다음 포커스가 있던 지점으로 이동
- <kbd>ctrl + alt + t</kbd>: Surround With... 선택한 코드를 제어문(if, while, try-catch 등)으로 감싸주는 기능

### 멀티 캐럿

Select Next Occurrence 없는 줄 아랏네 😂

- <kbd>alt + j</kbd>: 드래그한 단어 기준 다음 단어에 캐럿 추가
- <kbd>alt + shift + j</kbd>: 캐럿 추가한 거 하나씩 취소
- <kbd>ctrl + alt + shift + j</kbd>: 선택한 단어와 동일한 모든 위치에 캐럿 추가

### 빌드, 실행

- <kbd>ctrl + f2</kbd>: 실행 중인 앱 중단
- <kbd>ctrl + f9</kbd>: 빌드하기
- <kbd>ctrl + f10</kbd>: Update Running Application 런타임이 끝나지 않은 애플리케이션에 어떻게 할 지 묻는 대화창이 나타남
- <kbd>shift + f9</kbd>: Debug 모드로 시작
- <kbd>shift + f10</kbd>: Run 모드로 시작
- <kbd>ctrl + shift + f9</kbd>: 누구를 Debug 모드로 시작할지 묻는 대화창이 나타남
- <kbd>ctrl + shift + f10</kbd>: 누구를 Run 모드로 시작할지 묻는 대화창이 나타남
