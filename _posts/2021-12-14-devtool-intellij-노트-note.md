---
layout: post
date: 2021-12-14 18:23:22 +0900
title: '[devtool] IntelliJ 노트'
categories:
  - devtool
tags:
  - devtool
  - intellij
  - note
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.jetbrains.com/help/idea/getting-started.html](https://www.jetbrains.com/help/idea/getting-started.html)

#### 테스트 환경 정보

- IntelliJ IDEA 2021.3


## 개요

인텔리제이 관련 단축키와 설정 등의 간단 정리 글.

자바 개발자들 사이에선 인텔리제이 한 번 써보면 다시는 이클립스로 못돌아간다는 전설이 있다고 하더라. ~~뻥임~~


## Ultimate와 Community 버전 간 차이

지원하는 언어나 프레임워크에서 차이가 많이 난다. 몇 개만 꼽자면 Community 버전은 Spring, Java EE, JavaScript, TypeScript, Node.js, PHP, SQL 등을 미지원. 사실 순수 자바 프로젝트는 그냥 커뮤니티 버전 써도 됨. (JS 같은 건 VSCODE로 한다 치고)

자세한 내용은 [여기](https://www.jetbrains.com/products/compare/?product=idea&product=idea-ce)에.


## 코드의 빨간 밑줄이 안 없어진다면...

JetBrains 제품군에서 종종 발생하는 문제다. 대체로 IDE를 재시작하면 해결되지만, 이게 귀찮은 경우 메뉴의 `Code > Inspect Code`를 실행하면 해소되는 경우가 있다.

이래도 안되면 재시작... 🥲


## IDE log 파일 위치

[https://intellij-support.jetbrains.com/hc/en-us/articles/207241085-Locating-IDE-log-files](https://intellij-support.jetbrains.com/hc/en-us/articles/207241085-Locating-IDE-log-files)

요런 `C:\Users\fixal\AppData\Local\JetBrains\IntelliJIdea2022.3\log` 경로에 있는데 정확히 어디에 있는지는 Help 메뉴의 Show Log In Explorer를 누르면 나옴.

검색으로 해결 안 되는 문제(프리징 현상이라던지)가 발생하면 [지원 페이지](https://intellij-support.jetbrains.com/)로 가서 이 파일을 첨부해 문의하면 된다.


## 빌드나 런타임 에러가 발생하면

Project Structure(<kbd>ctrl + alt + shift + s</kbd>)에서 Modules 항목 설정은 이렇게 돼있는지 우선 확인:

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

로컬 서버로 톰캣을 별도로 추가해서 사용할 때만 해당. **TODO** 스프링 부트같은 내장 WAS는 확인 필요

일단 별도 설정 없이도 디버그 모드라면 Update Running Application(<kbd>ctrl + f10</kbd>)을 누르면 된다.

그런데 귀찮으니께, Run/Debug Configurations(<kbd>alt + u, r</kbd>)에서 'On frame deactivation' 항목의 값을 'Update classes and resources'로 변경한다. 이렇게 하면 인텔리제이가 포커스를 잃을 때마다 (필요한 경우 빌드 후) 자동으로 리로드한다.

JSP나 HTML 등의 리소스 파일은 바로 확인할 수 있고 서버 실행 모드에 따른 차이가 없다. 하지만 Java 클래스의 경우 일단 빌드하는 시간이 필요하기도 하고, 앱을 디버그 모드로 실행시키지 않으면 갱신하지 않는다. (Run 모드여도 빌드는 하는 것 같은디...)

검색해보니 ['File Watcher' 플러그인을 쓰는 방법](https://stackoverflow.com/questions/22713104/intellij-automatically-update-resources)도 있긴 함.


## 인텔리제이에서 사용할 VM options 지정하기

Find Action(<kbd>ctrl + shift + a</kbd>)에서 'Edit Custom VM Options'을 실행하면 파일이 하나 열리는데 여기에 입력하면 됨.


## 프로젝트별 배경 화면 다르게

원래는 테마를 프로젝트별로 설정하려고 했으나 현재(2023-07-09) 지원하지 않는다. 

우선 식별용 이미지 몇 장을 적당히 마련한 뒤, Find Action(<kbd>ctrl + shift + a</kbd>)에서 `Set Background Image`를 실행한다. `This project only` 항목에 체크하고 이미지를 설정하면 끝.


## File and Code Templates

평소 설정 그대로 붙여놓음

#### Class

```java
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};

#end
#parse("File Header.java")

import lombok.extern.slf4j.Slf4j;

/**
 * class description
 * 
 * @author ${USER}
 * @since ${DATE}
 */
@Slf4j
public class ${NAME} {

}

```

#### JavaScript, JSX, TypeScript, TypeScript JSX

```js
/**
 * @file file description
 * @author ${USER}
 * @since ${DATE}
 */

```


## 자동 완성

### Code Completion

**TODO**

### Postfix Completion

규칙대로 입력한 뒤 'Code Completion'을 발동하면 미리 작성한 코드가 자동으로 입력된다.

**TODO**

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
- `$SELECTION$`: 특정 코드를 선택(드래그)한 뒤 Surround With(<kbd>ctrl + alt + t</kbd>)로 라이브 템플릿을 선택하면 지정한 위치에 선택했던 코드가 자동으로 입력됨


## 외부 톰캣 사용하기

우선 번들 플러그인 Tomcat and TomEE를 활성화해야 하면 실행 설정으로 톰캣을 추가할 수 있다.

### 톰캣 퍼블리싱 폴더는 어디일까

`shell:local appdata\JetBrains\IntelliJIdeaXXXX.X\tomcat\36자리-의-랜덤-한-번호`

### Artifacts

빌드 프로세스에서 생성된 배포 가능한 결과 즉, 프로젝트를 실행하거나 배포하기 위해 필요한 파일들을 JAR나 WAR 따위로 패키징한 것을 말함. 별도로 설정하지 않으면 IntelliJ에서는 `out/` 디렉터리에 저장된다. 

exploded artifacts는 이 패키징 파일의 압축 해제 상태를 의미한다.

톰캣 플러그인과 외부 톰캣을 사용한다면 `Run/Debug Configurations`에서 배포할 artifact를 지정해야 하며, IntelliJ는 artifact를 Tomcat의 webapps 디렉토리에 복사하거나 참조하도록 설정된다. `out/artifacts/` 디렉터리가 이 과정에서 사용된다.

### Modules

프로젝트의 작업 단위 혹은 구성 단위를 의미한다. 소스 코드, 리소스, 라이브러리, 빌드 설정 등을 포함하는 논리적 단위다. `Project Structure`에서 설정하며, 각 모듈은 소스 폴더, 의존성, 컴파일 출력 경로 등을 정의한다.

모듈은 소스 코드를 컴파일해서 클래스 파일이나 리소스를 생성하는 데 사용된다. 예를 들어, 웹 앱 프로젝트라면 모듈은 자바 클래스, JSP, HTML, CSS 등을 포함하고, 이를 컴파일해서 `out/production/` 또는 `out/classes/`에 결과물을 만든다. 하나의 프로젝트에는 여러 모듈이 있을 수 있다.

### Artifacts는 Modules를 기반으로 생성된다

Artifacts는 모듈의 컴파일된 결과물(클래스 파일, 리소스 등)과 추가 설정(예: 라이브러리, WEB-INF 구조)을 패키징해서 배포 가능한 형태(WAR, JAR, exploded WAR 등)로 만든다.

예를 들어, 웹 앱 모듈이 있으면 그 모듈의 소스와 리소스를 컴파일한 뒤, WEB-INF, lib, 정적 파일 등을 포함해 WAR 파일이나 exploded WAR로 artifact를 만든다.

`Project Structure`에서의 차이는 다음과 같다:

- Modules: 소스 코드와 의존성을 정의
- Artifacts: 모듈의 출력물을 어떻게 패키징할지 정의(예: WAR로 묶을지, 어떤 파일을 포함할지)

그리고 `Run/Debug Configurations`에서의 차이는 다음과 같다:

- Modules: 프로젝트의 모듈 목록이 나타나며, 어떤 모듈을 빌드하고 실행할지 선택할 수 있다. 예를 들어, 웹 모듈을 선택하면 해당 모듈의 소스가 컴파일된다.
- Artifacts: 배포할 최종 출력물을 선택한다. 예를 들어, `프로젝트명:war` 또는 `프로젝트명:war exploded`를 선택해서 Tomcat에 배포한다. 이 artifact는 모듈의 컴파일 결과와 설정을 포함한다.

### 그럼 배포할 Artifact로 뭘 골라야 하는가?

보통은 hot deploy가 가능하고 배포 속도가 빠른 exploded artifact를 선택한다. 만약 둘 다 선택할 경우, 기본적으로 exploded 쪽이 사용되긴 하나 설정에 따라 달라질 수 있다.


## 추천 플러그인

- ⭐MoveTab: 단축키로 탭 이동하고 싶으면 설치. 단축키는 Move Tab Left/Right 찾아서 <kbd>shift + ctrl + alt + pageup/pagedown</kbd>으로 변경
- ⭐CamelCase: 카멜, 케밥, 스네이크 등 케이스 변환 지원. 기본 단축키: <kbd>shift + alt + u</kbd>
- Grep Console: 콘솔 로그에 색을 입히거나 필터링하는 플러그인. 인터페이스가 좀 복잡하긴 한데 쓸만함
- ⭐Emmet: HTML 태그를 단축어로 작성할 수 있게 해줌
- Extra Actions: 기본 인텔리제이에 없는 추가 기능을 제공하는 플러그인이다. (사실 캐럿 추가 기능 하나 때문에 쓰는 거)
  - 에디터에서 선택한 영역의 각 줄에 캐럿 추가
  - 다음 paragraph(단락, 문단)에 캐럿 추가
  - 따옴표 감싸기 토글
  - String 리터럴을 더하기 연산자로 자르기
- GitHub Copilot: AI가 코드를 작성해주는 🐶쩌는 플러그인
  - <kbd>ctrl + alt + shift + o</kbd>: 활성화/비활성화 토글
  - <kbd>alt + \ </kbd>: 코파일럿 자동 완성 발동인데, 이 키가 IntelliJ 2022.3 버전의 Show Collapsed Main Menu 명령과 충돌하니 삭제해줘야 함
  - <kbd>tab</kbd>: 코파일럿 제안 선택
  - <kbd>alt + [</kbd>: 다음 제안 보기
  - <kbd>alt + ]</kbd>: 이전 제안 보기
  - <kbd>ctrl + right</kbd>: 제안을 단어 단위로 적용하기
  - <kbd>ctrl + alt + right</kbd>: 제안을 줄 단위로 적용하기
  - <kbd>ctrl + alt + shift + \ </kbd>: Tool Windows > GitHub Copilot. 코파일럿 창 열기. 이 창에선 코드 자동 완성 추천 목록이 나온다. (원래 키에서 변경)
  - <kbd>alt + p</kbd>: Tool Windows > GitHub Copilot Chat. 코파일럿 채팅 창 열기 (원래 키에서 변경)
  - <kbd>alt + shift + p</kbd>: Plugins > GitHub Copilot > Inline Chat. 에디터 인라인으로 코파일럿 채팅 창 열기 (원래 키에서 변경)


## 작성자 저장용 단축키 설정

- <kbd>ctrl + alt + shift + e</kbd>: File Open Actions > Recent Project. 최근 열었던 프로젝트 열기
- <kbd>f1</kbd>: Help > Find Action. 모든 명령 검색창인데 f1이 원래 도움말이었던거 지워버리고 요 키도 추가함
- <kbd>ctrl + z</kbd>: Edit > Undo. 되돌리기. 다른 키는 다 지움
- <kbd>ctrl + y</kbd>: Edit > Redo. 다시 되돌리기. 다른 키는 다 지움
- <kbd>ctrl + shift + d</kbd>: Editor Actions > Delete Line. 라인 삭제. 기존 키 매핑은 삭제
- <kbd>ctrl + alt + up</kbd>: Editor Actions > Clone Caret Above. 위로 멀티 캐럿 생성. 다른 키는 지움
- <kbd>ctrl + alt + down</kbd>: Editor Actions > Clone Caret Below. 아래로 멀티 캐럿 생성. 다른 키는 지움
- <kbd>ctrl + shift + k</kbd>: Editor Actions > Duplicate Line or Selection. 중복 라인 생성. 기존 다른 명령의 키 매핑은 삭제
- <kbd>ctrl + pageup</kbd>: Editor Tabs > Select Previous Tab. 이전 탭으로 이동. 기존 다른 명령의 키 매핑은 삭제
- <kbd>ctrl + pagedown</kbd>: Editor Tabs > Select Next Tab. 다음 탭으로 이동. 기존 다른 명령의 키 매핑은 삭제(캐럿을 현재 화면 내 맨 위나 아래로 이동인데 잘 안써서 삭제함)
- <kbd>ctrl + alt + shift + '</kbd>: Editor Tabs > Maximize Editor/Normalize Splits. 에디터 창 최대화/원래대로 토글
- <kbd>alt + left</kbd>: Navigate > Back. 이전 포커스 지점으로 이동. 다른 키 매핑은 내비둠
- <kbd>alt + right</kbd>: Navigate > Forward. 다음 포커스 지점으로 이동. 다른 키 매핑은 내비둠
- <kbd>alt + s</kbd>: Database > Attach Session. 데이터베이스 연결 선택하는 기능
- <kbd>alt + z</kbd>: Active Editor > Soft-Wrap
- <kbd>alt + x</kbd>: Other > Clear text. 콘솔 지우기
- <kbd>alt + w</kbd>: Debugger Actions > Add to Watches. 디버그 모드에서 지켜볼 표현식 영역에 추가
- <kbd>ctrl + \ </kbd>: Navigate > Goto by Reference Actions > File Structure. 기존 단축키는 *Root directory* 인데, 어차피 잘 안쓰니 삭제
- <kbd>alt + shift + = </kbd> <kbd>alt + shift + - </kbd>: Main Menu > Window > Editor Tabs > Split Right/Down. 에디터를 수평/수직으로 분할하는 기능이다. 해당 키 조합의 기본값 Zoom in/out은 지움
- <kbd>shift + f12</kbd>: Main Menu > Window > Tool Window Layouts > Restore Current Layout. 기껏 변경한 레이아웃 되돌리는 단축키니까 지우자.
- <kbd>end</kbd>: Code > Code Completion > Insert Inline Proposal's Line. 언젠가부터 자동완성 제안의 라인 단위 수락 키로 추가됐는데 안지우면 🐶불편함.


## 기본 단축키

### 멀티 캐럿

인텔리제이에선 Add Caret 혹은 Select Next Occurrence 쯤으로 부른다.

- <kbd>alt + j</kbd>: Add Selection for Next Occurrence. 드래그한 단어 기준 다음 단어에 캐럿 추가
- <kbd>alt + shift + j</kbd>: Unselect Occurrence. 캐럿 추가한 거 하나씩 취소
- <kbd>ctrl + alt + shift + j</kbd>: Select All Occurrences. 선택한 단어와 동일한 모든 위치에 캐럿 추가
- <kbd>alt + shift + g</kbd>: Add Carets to Ends of Selected Lines. 선택한 모든 라인에 캐럿 생성

### 전역

- <kbd>esc</kbd>: 파일 에디터로 포커스
- <kbd>alt + insert</kbd>: 새로 추가. 상황에 따라 구성 요소/파일/rule 등을 추가하는 메뉴를 보여줌.
- <kbd>shift, shift</kbd>: Search Everywhere 모든 것(?) 열기
- <kbd>ctrl, ctrl</kbd>: Run Anything 모든 것(?) run하기
- <kbd>ctrl + alt + s</kbd>: Settings 인텔리제이 설정
- <kbd>ctrl + alt + shift + s</kbd>: Project Structure 현재 프로젝트 전용 설정
- <kbd>ctrl + shift + '</kbd>: 포커스가 있는 창 최대화(안 되는 것도 있다)
- <kbd>ctrl + shift + a</kbd>: 실행 가능한 모든 명령 검색창
- <kbd>ctrl + shift + n</kbd>: Go to File 파일 열기
- <kbd>ctrl + e</kbd>: Recent Files 최근 파일 목록 보기
- <kbd>ctrl + shift + e</kbd>: Recently Changed Files 최근 변경된 파일 목록 보기
- <kbd>ctrl + f4</kbd>: 창 닫기
- <kbd>alt + home</kbd>: 파일 트리 탐색으로 포커스
- <kbd>ctrl + shift + f</kbd>: Find in Files 파일 내용으로 검색
- <kbd>ctrl + shift + f12</kbd>: Hide All Tool Windows
- <kbd>ctrl + shift+ \ </kbd>: Go to URL Mapping

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

- <kbd>ctrl + \[/\]</kbd>: Move Caret to Code Block Start/End. 코드 블록의 처음 혹은 끝으로 이동하는 기능
- <kbd>ctrl + shift + \[/\]</kbd>: Move Caret to Code Block Start/End with Selection. 코드 블록의 처음 혹은 끝으로 이동하면서 그 사이의 코드를 선택한다.
- <kbd>ctrl + q</kbd>: Quick Documentation 퀵 뷰 창에서 서식이 적용된 자바독 보기
- <kbd>ctrl + shift + i</kbd>: Quick Definition. 툴팁창으로 선언부 보기
- <kbd>ctrl + b</kbd>: 정의된 파일이나 선언부로 이동, 이미 선언부일 땐 참조하는 코드 미리보기
- <kbd>ctrl + alt + b</kbd>: (인터페이스의) 구현부가 따로 있으면 그 쪽으로, 아니면 선언부로 이동
- <kbd>ctrl + u</kbd>: 오버라이딩 메서드의 super 메서드로 이동
- <kbd>alt + enter</kbd>: Show Context Actions 파일 에디터에서 발동하면 상황에 맞는 메뉴 보여줌. 대부분 리팩터링 관련.
- <kbd>alt + f1</kbd>: Select in/ 어느 윈도우에서 현재 파일(혹은 포커스가 있는 요소)을 보여줄 지 선택하는 창이 열림. <kbd>alt + f1, 1</kbd> 누르면 프로젝트 윈도우에서 현재 파일이 보이는 식.
- <kbd>alt + f7</kbd>: Find Usages 포커스된 대상이 어디서 쓰이고 있는지 프로젝트 전체 검색
- <kbd>ctrl + alt + f7</kbd>: Show Usages 대상을 참조하고 있는 코드를 퀵 뷰 창에서 보여줌
- <kbd>ctrl + f7</kbd>: Find Usages in File/ 포커스된 대상이 어디서 쓰이고 있는지 현재 파일 내 검색. <kbd>ctrl + shift + f7</kbd>은 Highlight Usages in File인데, 인텔리제이 2020.1 버전부터 두 기능 간 차이가 없다고 한다. [JetBrains Support 답변](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360008113359--Find-usages-in-file-vs-Highlight-usages-in-file-)
- <kbd>ctrl + f1</kbd>: Error Description. 에러 툴팁 보기
- <kbd>ctrl + f12</kbd>: File Structure. eclipse의 빠른 아웃라인 보기 기능과 같음.
- <kbd>ctrl + alt + l</kbd>: 오토 fㅗ매팅
- <kbd>ctrl + shift + backspace</kbd>: 마지막 수정 지점으로 이동
- <kbd>shift + f4</kbd>: Open source in new window. 현재 파일 새 창에서 보기
- <kbd>f2</kbd> <kbd>shift + f2</kbd>: Highlighted Error. 다음/이전 에러 지점으로 이동
- <kbd>ctrl + alt + 방향키좌우</kbd>: 이전/다음 포커스가 있던 지점으로 이동
- <kbd>ctrl + alt + t</kbd>: Surround With. 선택한 코드를 제어문(if, while, try-catch 등)으로 감싸주는 기능
- <kbd>ctrl + alt + shift + t</kbd>: Refactor This. 캐럿 위치 기준으로 소스 리팩토링 하기
- <kbd>ctrl + j</kbd>: Insert Live Template. 라이브 템플릿 삽입하기
- <kbd>ctrl + alt + j</kbd>: Surround With Live Template. 선택한 텍스트를 라이브 템플릿으로 감싸
- <kbd>ctrl + alt + q</kbd>: Toggle Rendered View. 자바독 편집/읽기 모드 토글
- <kbd>ctrl + shift + t</kbd>: Go To Test. 현재 클래스의 테스트 클래스로 이동하거나 되돌아옴
- <kbd>ctrl + h</kbd>: Type Hierarchy. 타입 계층 보기
- <kbd>ctrl + shift + h</kbd>: Method Hierarchy. 메서드 계층 보기
- <kbd>ctrl + alt + h</kbd>: Call Hierarchy. 호출 계층 보기
- <kbd>ctrl + alt + shift + left/right</kbd>: Move Element Left/Right. 메서드 매개변수의 순서를 변경할 때 사용함.
- <kbd>alt + shift + c</kbd>: Recent changes. 파일 시스템의 최근 변경 목록을 보여주는 것 같은데, 어떤 기준으로 나오는지 잘 모르겠음 🤔
- <kbd>ctrl + shift + l</kbd>(서브라임 스타일): Split Selection into Lines. Extra Actions 플러그인을 설치해야 사용 가능. 기존 바인딩은 삭제
- <kbd>alt + shift + l</kbd>: Save Context. 컨텍스트를 저장한다. 컨텍스트는 열려있는 파일들을 의미하는 모양
- <kbd>alt + shift + s</kbd>: Load Context. 저장한 컨텍스트를 불러온다.
- <kbd>ctrl + shift + del</kbd>: **TODO**

### 복사/붙이기

- <kbd>ctrl + alt + shift + c</kbd>: Copy Reference. 현재 커서/캐럿/포커스의 위치를 기준으로 상대 경로를 복사한다. 예를 들어자바 클래스의 메서드에 캐럿을 두고 누르면 `패키지/클래스명#메서드명`의 형태로 복사한다. 심볼을 특정할 수 없는 경우엔 `디렉터리/파일명:라인번호`의 형태로 복사한다.
- <kbd>ctrl + shift + c</kbd>: Copy Absolute Path. 현재 파일의 (윈도우에선 드라이브 문자 `C:`, `D:` 부터 시작하는) 절대 경로를 복사한다.

### 빌드, 실행

- <kbd>ctrl + f2</kbd>: 실행 중인 앱 중단
- <kbd>ctrl + f9</kbd>: 빌드하기
- <kbd>ctrl + f10</kbd>: Update Running Application. 런타임이 끝나지 않은 애플리케이션에 어떻게 할 지 묻는 대화창이 나타남. 근데 2023년 3월부터 자꾸 하라는 업데이트는 안하고 IME 툴팁이 나타나서 <kbd>ctrl + alt + \ </kbd>도 추가함. (윈도우 11 문제 같은데, 어떻게 하면 또 풀린다.)
- <kbd>shift + f9</kbd>: Debug 모드로 시작
- <kbd>shift + f10</kbd>: Run 모드로 시작
- <kbd>ctrl + shift + f9</kbd>: 어떤 것을 Debug 모드로 시작할지 묻는 대화창이 나타남
- <kbd>ctrl + shift + f10</kbd>: 어떤 것을 Run 모드로 시작할지 묻는 대화창이 나타남

### 디버깅

- <kbd>alt + f10</kbd>: Show Execution Point. 디버깅 중, 브레이크 포인트에 의해 실행을 정지하고 있는 지점으로 포커스를 이동하는 기능이다.

