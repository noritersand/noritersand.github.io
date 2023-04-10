---
layout: post
date: 2021-12-14 18:23:22 +0900
title: '[devtool] IntelliJ 노트'
categories:
  - eclipse
tags:
  - devtool
  - intellij
  - note
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://www.jetbrains.com/help/idea/getting-started.html](https://www.jetbrains.com/help/idea/getting-started.html)

#### 버전 정보

- IntelliJ IDEA 2021.3


## 개요

인텔리제이 관련 단축키와 설정 등의 간단 정리 글.

자바 개발자들 사이에선 인텔리제이 한 번 써보면 다시는 이클립스로 못돌아간다는 전설이 있다고 하더라. ~~뻥임~~


## Ultimate와 Community 버전 간 차이

지원하는 언어, 프레임웍에서 차이가 많이 난다. 몇 개만 꼽자면 Community 버전은 Spring, Java EE, JavaScript, TypeScript, Node.js, PHP, SQL 등을 미지원. 사실 순수 자바 프로젝트는 그냥 커뮤니티 버전 써도 됨. (JS 같은 건 VSCODE로 한다 치고)

자세한 내용은 [여기](https://www.jetbrains.com/products/compare/?product=idea&product=idea-ce)에.


## TODO 톰캣 퍼블리싱 폴더는 어디일까

이클립스의 고것과 같은 경로를 못찾겠다. 설마 target을 직접 보는건지.


## IDE log 파일 위치

[https://intellij-support.jetbrains.com/hc/en-us/articles/207241085-Locating-IDE-log-files](https://intellij-support.jetbrains.com/hc/en-us/articles/207241085-Locating-IDE-log-files)

요런 `C:\Users\fixal\AppData\Local\JetBrains\IntelliJIdea2022.3\log` 경로에 있는데 정확히 어디에 있는지는 Help 메뉴의 Show Log In Explorer를 누르면 나옴.

검색으로 해결 안되는 문제(프리징 현상이라던지)가 발생하면 [지원 페이지](https://intellij-support.jetbrains.com/)로 가서 이 파일을 첨부해 문의하면 된다.


## 빌드나 런타임 에러가 발생하면

Project Structure<kbd>ctrl + alt + shift + s</kbd>에서 Modules 항목 설정은 이렇게 돼있는지 우선 확인:

![](/images/intellij-project-settings-2.png)


## 한글 깨짐 문제

일단 발견한 인코딩 관련 설정은 요렇게 있다.

#### \#1 VM Option: file.encoding

만약 WAS라면: Run/Debug Configurations 혹은 Services의 WAS 설정으로 이동해서 VM options에 `-Dfile.encoding=UTF-8` 추가

아니면: Edit Custom VM Options로 이동(<kbd>ctrl + shift + a</kbd> 후 검색)한 뒤 `-Dfile.encoding=UTF-8` 추가

#### \#2 에디터 인코딩 

Settings > Editor > File Encodings로 이동한 뒤:

- Project Encoding을 UTF-8로 변경
- Default encoding for properties files를 UTF-8, 그 옆에 Transparent native-to-ascii conversion 체크

#### \#3 콘솔 출력 인코딩

`Settings > Editor > General > Console`로 이동해서 Default Encoding을 UTF-8로 변경


## 런타임 중 변경된 파일 자동으로 다시 불러오기

Hot deploy, Hot swap, Hot code replace 등으로 불리는(엄밀히 따지면 셋 다 다르지만...), 런타임 중 변경된 파일을 즉시 교체하는 기능이다.

로컬 서버로 톰캣을 별도로 추가해서 사용할 때만 해당. **TODO 스프링 부트같은 내장 WAS는 확인 필요.**

일단 별도 설정 없이도 디버그 모드라면 Update Running Application<kbd>ctrl + f10</kbd>을 누르면 된다.

그런데 귀찮으니께, Run/Debug Configurations<kbd>alt + u, r</kbd>에서 'On frame deactivation' 항목의 값을 'Update classes and resources'로 변경한다. 이렇게 하면 인텔리제이가 포커스를 잃을 때마다 (필요한 경우 빌드 후) 자동으로 리로드한다.

JSP나 HTML 등의 리소스 파일은 바로 확인할 수 있고 서버 실행 모드에 따른 차이가 없다. 하지만 Java 클래스의 경우 일단 빌드하는 시간이 필요하기도 하고, 앱을 디버그 모드로 실행시키지 않으면 갱신하지 않는다. (Run 모드여도 빌드는 하는 것 같은디...)

검색해보니 ['File Watcher' 플러그인을 쓰는 방법](https://stackoverflow.com/questions/22713104/intellij-automatically-update-resources)도 있긴 함.



## 인텔리제이에서 사용할 VM options 지정하기

Find Action<kbd>ctrl + shift + a</kbd>에서 'Edit Custom VM Options'을 실행하면 파일이 하나 열리는데 여기에 입력하면 됨.


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

`Settings > Editor > File and Code Templates`

### Live Templates

이클립스에서 특정 키워드 후의 자동 완성과 같은 기능이다.

`Settings > Editor > Live Templates`으로 이동해서 그룹 선택 후 우측의 십자 모양 아이콘 클릭한 다음 요딴식으로 작성한다:

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
- `$SELECTION$`: 특정 코드를 선택(드래그)한 뒤 Surround With<kbd>ctrl + alt + t</kbd>로 라이브 템플릿을 선택하면 지정한 위치에 선택했던 코드가 자동으로 입력됨


## 추천 플러그인

- MoveTab: 단축키로 탭 이동하고 싶으면 설치. 단축키는 Move Tab Left/Right 찾아서 <kbd>shift + ctrl + alt + pageup/pagedown</kbd>으로 변경
- CamelCase: 카멜, 케밥, 스네이크 등 케이스 변환 지원. 기본 단축키: <kbd>shift + alt + u</kbd>

#### GitHub Copilot

AI가 코드를 작성해주는 쩌는 플러그인. 단축키는:

- 활성화/비활성화 토글: <kbd>ctrl + alt + shift + o</kbd>
- 코파일럿 발동: <kbd>alt + \\</kbd> 인데, 이 키가 IntelliJ 2022.3 버전의 Show Collapsed Main Menu 명령과 충돌하니 삭제해줘야 함.
- 코파일럿 제안 선택: <kbd>tab</kbd>
- 다음 제안 보기: <kbd>alt + [</kbd>
- 이전 제안 보기: <kbd>alt + ]</kbd>


## 작성자 저장용 단축키 설정

- File Open Actions > Recent Project: <kbd>ctrl + alt + shift + e</kbd> 최근 열었던 프로젝트 열기
- Help > Find Action: <kbd>f1</kbd> 모든 명령 검색창인데 f1이 원래 도움말이었던거 지워버리고 요 키도 추가함
- Edit > Undo: <kbd>ctrl + z</kbd>: 되돌리기. 다른 키는 다 지움
- Edit > Redo: <kbd>ctrl + y</kbd>: 다시 되돌리기. 다른 키는 다 지움
- Editor Actions > Delete Line: <kbd>ctrl + shift + d</kbd> 라인 삭제. 기존 키 매핑은 삭제
- Editor Actions > Clone Caret Above: <kbd>ctrl + alt + up</kbd> 위로 멀티 캐럿 생성. 다른 키는 지움
- Editor Actions > Clone Caret Below: <kbd>ctrl + alt + down</kbd> 아래로 멀티 캐럿 생성. 다른 키는 지움
- Editor Actions > Duplicate Line or Selection: <kbd>ctrl + shift + k</kbd> 중복 라인 생성. 기존 다른 명령의 키 매핑은 삭제
- Editor Tabs > Select Previous Tab: <kbd>ctrl + pageup</kbd> 이전 탭으로 이동. 기존 다른 명령의 키 매핑은 삭제
- Editor Tabs > Select Next Tab: <kbd>ctrl + pagedown</kbd> 다음 탭으로 이동. 기존 다른 명령의 키 매핑은 삭제(캐럿을 현재 화면 내 맨 위나 아래로 이동인데 잘 안써서 삭제함)
- Editor Tabs > Maximize Editor/Normalize Splits: <kbd>ctrl + alt + shift + '</kbd> 에디터 창 최대화/원래대로 토글
- Navigate > Back: <kbd>alt + left</kbd> 이전 포커스 지점으로 이동. 다른 키 매핑은 내비둠
- Navigate > Forward: <kbd>alt + right</kbd> 다음 포커스 지점으로 이동. 다른 키 매핑은 내비둠
- Debugger Actions > Add to Watches: <kbd>alt + w</kbd> 디버그 모드에서 지켜볼 표현식 영역에 추가
- Database > Attach Session: <kbd>alt + s</kbd>로 단축키 추가. 데이터베이스 연결 선택하는 기능임
- Active Editor > Soft-Wrap: <kbd>alt + z</kbd>
- Other > Clear text: <kbd>alt + x</kbd>: 콘솔 지우기


## 기본 단축키

### 전역

- <kbd>esc</kbd>: 파일 에디터로 포커스
- <kbd>alt + insert</kbd>: 새로 추가. 상황에 따라 구성요소/파일/rule 등을 추가하는 메뉴를 보여줌.
- <kbd>shift, shift</kbd>: Search Everywhere 모든 것(?) 열기
- <kbd>ctrl, ctrl</kbd>: Run Anything 모든 것(?) run하기
- <kbd>ctrl + alt + s</kbd>: Settings 인텔리제이 설정
- <kbd>ctrl + alt + shift + s</kbd>: Project Structure 현재 프로젝트 전용 설정
- <kbd>ctrl + shift + '</kbd>: 포커스가 있는 창 최대화(안되는 것도 있다)
- <kbd>ctrl + shift + a</kbd>: 실행 가능한 모든 명령 검색창
- <kbd>ctrl + shift + n</kbd>: Go to File 파일 열기
- <kbd>ctrl + e</kbd>: Recent Files 최근 파일 목록 보기
- <kbd>ctrl + shift + e</kbd>: Recently Changed Files 최근 변경된 파일 목록 보기
- <kbd>ctrl + f4</kbd>: 창 닫기
- <kbd>alt + home</kbd>: 파일 트리 탐색으로 포커스
- <kbd>ctrl + shift + f</kbd>: Find in Files 파일 내용으로 검색
- <kbd>ctrl + shift + f12</kbd>: Hide All Tool Windows
- <kbd>ctrl + shift+ \\</kbd>: Go to URL Mapping

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
- <kbd>ctrl + alt + shift + left/right</kbd>: Stretch to Left/Right. 창을 좌측 혹은 우측으로 늘리거나 줄임. 코드 에디터 화면에선 (가능할 경우에만) Move Element Left/Right 기능으로 작동한다.

### 코드 에디터

- <kbd>ctrl + q</kbd>: Quick Documentation. 툴팁창으로 자바독 보기
- <kbd>ctrl + b</kbd>: 정의된 파일이나 선언부로 이동, 이미 선언부일 땐 참조하는 코드 미리보기
- <kbd>ctrl + alt + b</kbd>: (인터페이스의) 구현부가 따로 있으면 그 쪽으로, 아니면 선언부로 이동
- <kbd>alt + enter</kbd>: Show Context Actions/ 파일 에디터에서 발동하면 상황에 맞는 메뉴 보여줌. 대부분 리팩터링 관련.
- <kbd>alt + f1</kbd>: Select in/ 어느 윈도우에서 현재 파일(혹은 포커스가 있는 요소)을 보여줄 지 선택하는 창이 열림. <kbd>alt + f1, 1</kbd> 누르면 프로젝트 윈도우에서 현재 파일이 보이는 식.
- <kbd>alt + f7</kbd>: Find Usages/ 포커스된 대상이 어디서 쓰이고 있는지 프로젝트 전체 검색
- <kbd>ctrl + f7</kbd>: Find Usages in File/ 포커스된 대상이 어디서 쓰이고 있는지 현재 파일 내 검색. <kbd>ctrl + shift + f7</kbd>은 Highlight Usages in File인데 뭔 차이인지 모르겠다.
- <kbd>ctrl + f1</kbd>: Error Description. 에러 툴팁 보기
- <kbd>ctrl + f12</kbd>: File Structure. eclipse의 빠른 아웃라인 보기 기능과 같음.
- <kbd>ctrl + alt + l</kbd>: 오토 fㅗ매팅
- <kbd>ctrl + shift + backspace</kbd>: 마지막 수정 지점으로 이동
- <kbd>shift + f4</kbd>: Open source in new window. 현재 파일 새 창에서 보기
- <kbd>f2</kbd> <kbd>shift + f2</kbd>: Highlighted Error. 다음/이전 에러 지점으로 이동
- <kbd>ctrl + alt + 방향키좌우</kbd>: 이전/다음 포커스가 있던 지점으로 이동
- <kbd>ctrl + alt + t</kbd>: Surround With. 선택한 코드를 제어문(if, while, try-catch 등)으로 감싸주는 기능
- <kbd>ctrl + alt + q</kbd>: Toggle Rendered View. 자바독 편집/읽기 모드 토글
- <kbd>ctrl + shift + t</kbd>: Go To Test. 현재 클래스의 테스트 클래스로 이동하거나 되돌아옴
- <kbd>ctrl + h</kbd>: Type Hierarchy. 타입 계층 보기
- <kbd>ctrl + shift + h</kbd>: Method Hierarchy. 메서드 계층 보기
- <kbd>ctrl + alt + h</kbd>: Call Hierarchy. 호출 계층 보기
- <kbd>ctrl + alt + shift + left/right</kbd>: Move Element Left/Right. 메서드 파라미터의 순서를 변경할 때 사용함.

### 멀티 캐럿

Select Next Occurrence.

- <kbd>alt + j</kbd>: 드래그한 단어 기준 다음 단어에 캐럿 추가
- <kbd>alt + shift + j</kbd>: 캐럿 추가한 거 하나씩 취소
- <kbd>ctrl + alt + shift + j</kbd>: 선택한 단어와 동일한 모든 위치에 캐럿 추가
- <kbd>alt + shift + g</kbd> 선택한 모든 라인에 캐럿 생성

### 빌드, 실행

- <kbd>ctrl + f2</kbd>: 실행 중인 앱 중단
- <kbd>ctrl + f9</kbd>: 빌드하기
- <kbd>ctrl + f10</kbd>: Update Running Application. 런타임이 끝나지 않은 애플리케이션에 어떻게 할 지 묻는 대화창이 나타남. 근데 2023년 3월부터 자꾸 하라는 업데이트는 안하고 IME 툴팁이 나타나서 <kbd>ctrl + alt + \\</kbd>도 추가함. (윈도우 11 문제 같은데, 어떻게 하면 또 풀린다.)
- <kbd>shift + f9</kbd>: Debug 모드로 시작
- <kbd>shift + f10</kbd>: Run 모드로 시작
- <kbd>ctrl + shift + f9</kbd>: 어떤 것을 Debug 모드로 시작할지 묻는 대화창이 나타남
- <kbd>ctrl + shift + f10</kbd>: 어떤 것을 Run 모드로 시작할지 묻는 대화창이 나타남

### 디버깅

- <kbd>alt + f10</kbd>: Show Execution Point. 디버깅 중, 브레이크 포인트에 의해 실행을 정지하고 있는 지점으로 포커스를 이동하는 기능이다.

