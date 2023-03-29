---
layout: post
date: 2018-04-17 11:20:58 +0900
title: '[Eclipse] eclipse 초기 설정'
categories:
  - eclipse
tags:
  - devtool
  - eclipse
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://www.eclipse.org/downloads](http://www.eclipse.org/downloads)
- [http://tomcat.apache.org](http://tomcat.apache.org)
- [https://wiki.eclipse.org/Eclipse.ini](https://wiki.eclipse.org/Eclipse.ini)


## VM arguments 설정

### 파일 기본 인코딩

eclipse.ini에 `-Dfile.encoding=UTF-8` 추가

### user

eclipse.ini에 `-Duser.name=이름` 추가. 이클립스 VM arguments(=시스템 프로퍼티)명은 `user`임.

### vm 경로 지정 방법

eclipse.ini에 아래 내용 추가:

```
-vm
C:\Program Files\openjdk\jdk-10\bin\javaw.exe
```

`-vmargs` 위에 있지 않으면 적용 안될 수도 있다. 왜인지는 여백이 부족하여 적지 않음(?).


## 검색 창 설정 변경

- 검색 창<kbd>ctrl + h</kbd>에서 'Customize' 진입, 'Task'와 'Plug-in' 체크 해제.


## 파일 자동 갱신

`Window > Preferences > workspace` 우측 화면의 'Refresh using native hooks or pollings' 체크

이클립스는 기본적으로 이클립스 내에서 직접 변경하지 않은 파일은 변경을 감지하지 않는다. 이건 파일 변경 감지 기능을 켜는 옵션.


## 인코딩 환경 설정

얘네들을 'UTF-8'로 변경한다:

- `General > Workspace` 의 Text file encoding
- `Window > Preferences > Web > CSS Files`
- `Window > Preferences > Web > Jsp Files`
- `Window > Preferences > Web > HTML Files`


## 파일 확장자별 한글 인코딩을 UTF-8로 변경

- `Window > Preferences > General > Contents type` 우측 화면에서 원하는 항목을 선택하고, 'Default encoding'을 `utf-8`로 변경 후 'update' 버튼 클릭


## 자바독 자동 완성

`Window > Preferences > Java > Code Style > Code Templates` 메뉴로 이동하고 항목 중 Types의 Edit 창을 열어서 다음처럼 작성한다.

```java
/**
 * ${tags}
 * @since ${id:date('yyyy-MM-dd')}
 * @author ${user}
 */
```

`${date}` 태그는 기본포멧이 yyyy.mm.dd. 으로 되어 있다.

```
${date}  --> 2015. 7. 1.
```

만약 포맷을 yyyy-MM-dd로 바꾸고 싶다면 이클립스 실행파일과 같은 폴더에 있는 eclipse.ini 에 다음 줄을 추가하고 이클립스를 재시작한다.

```
-Duser.language=fr-CA
```

위 방법은 Neon.1a Release (4.6.1) 에선 통하지 않고 (2016-11-19 확인) 템플릿 변수 `${date}`를 다음처럼 작성해야 된다.

```
${id:date('yyyy-MM-dd')}
```


## Formatter

`Window > Preferences > Java > Code Style > Formatter` 메뉴로 이동, 'New...'를 클릭해서 새 프로파일을 생성한다.
이후 자동으로 프로파일 에딧창으로 이동되는데 여기서 'Comments' 탭으로 이동, `New line after @param tags`를 체크 해제한다.

![](/images/eclipse-1.png)

이 작업을 `JavaScript > Code Style > Formatter` 에서도 반복한다.


## Syntax Coloring

자바 메서드 호출 표현식이 눈에 잘 띄도록 변경한다. `Window > Preferences > Java > Editor > Syntax Coloring`에서 'Element' 목록 중 'Methods'와 'Inherited method invocations' 수정. **이클립스 버전에 따라 필요 없을 수도 있다.**


## JSP 템플릿

새로운 JSP 페이지를 작성 할 때 구성하는 템플릿 파일을 다음과 같이 변경 가능 하다. `Window > Perferences > Web > JSP Files > Editor > Template` 오른쪽 화면의 'New JSP File(html)' 을 선택 하고 'Edit' 버튼. 수정 후 'Ok'.

```html
<%@ page language="java" contentType="text/html; charset=${encoding}" pageEncoding="${encoding}"%>
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="${encoding}">
  <!-- if not HTML5 -->
  <!-- <meta http-equiv="Content-Type" content="text/html; charset=${encoding}"> -->
  <title></title>
</head>
<body>
${cursor}
</body>
</html>
```


## DEBUG 모드로 구동할 때 uncaught exception에서 브레이크 걸지 않기

- `Window > Preferences > Java > Debug > Suspend execution on uncaught exceptions` 체크 해제


## git 관련

### 로컬 저장소 기본 경로 변경

`Window > Preferences > Version Control > Git`에서 `Default repository folder`를 다른 경로로 변경한다. (ex: `C:\dev\git`)

### 커밋 작성자 글로벌 설정

`Window > Preferences > Version Control > Git > Configuration`에서 `user.name` 항목과 `user.email` 항목을 추가한다.

### auto crlf 저장소별 설정

[Git은 커밋할 때 자동으로 CRLF를 LF로 변환해주고 반대로 Checkout할 때 LF를 CRLF로 변환해 주는 기능이 있다. core.autocrlf 설정으로 이 기능을 켤 수 있다.](https://git-scm.com/book/ko/v1/Git%EB%A7%9E%EC%B6%A4-Git-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
이클립스에서 설정하는 방법은:
`Window > Preferences > Version Control > Git > Configuration`에서 `core.autocrlf=true`를 추가.


## eclipse-jee 버전에 포함된 안쓰는 기능(feature 혹은 software) 삭제

삭제해도 ~~될 것 같은~~ 되는 기능:

- Axis2
- CXF Web Services
- Dali Java Persistence Tools
- Data Tools Platform
- MyLyn
- JSF(Java Server Faces)
- JAX-WS

이 정도면 차라리 eclipse-java 버전에 필요한 기능만 설치하는게 나을 수도?

- Java EE Developer Tools: jsp로 검색하면 나옴
- Web Developer Tools: wst나 jst로 검색하면 나옴
- spring tool suite

요 정도.

remote system explorer operation 관련 프로세스 계속 띄우던 기능이 뭐였는지 까먹었음. RST? RSE?


## 추천 확장 기능

- ~~Eclipse Quicksearch~~: ~~STS의 기본기능인 Quicksearch와 같은 증분 검색(글자를 입력하는 도중에 계속 검색) 확장 기능. ~~검색 범위를 지정할 수 없기 때문에 활성화된 프로젝트와 파일 개수가 많을 수록 검색 속도가 느려지는 단점이 있다.~~ 최근 업데이트로 와일드 카드를 사용해서 파일명이나 확장자를 지정할 수 있도록 범위 설정 기능이 추가되었다. Spring Tools 확장 기능에 포함되어 있기도 하다.~~ 최근 버전에선(2019-09 확인) 플러그인 설치 없이 기본 기능으로 사용 가능.
- AnyEditTools: 잡다한 유틸리가 있는 확장 기능. 주로 표기법 치환(HTML 기호 <-> entities, camelCase <-> SCREAMING_SNAKE_CASE 등)용으로 쓰임. **설치 후 아래 설정할 것:**  
  - `Window > Preferences > General > Editors > AnyEdit Tools > Auto Convert`에서 'Remove trailing whitespace' 체크 해제
  - 키 설정에서 'AnyEdit Tools' 카테고리 중 'Convert Camel <-> Underscores', 'Convert Chars to Html Entities' **빼고 다** 삭제
- [eclipse-pmd](https://marketplace.eclipse.org/content/eclipse-pmd): 코드 정적분석/품질관리 툴이다. PMD 룰 자체에는 의문이 좀 있긴 하지만...
- [eclipse-pmd: getting-started](http://acanda.github.io/eclipse-pmd/getting-started.html)
- [SonarLint](https://www.sonarlint.org/): SonarQube의 이클립스판. PMD와 비슷한 코드 정적분석/품질관리 툴.
- [CodeMix](https://marketplace.eclipse.org/content/codemix): 파이썬, PHP, 자바스크립트 등을 빠와하게 지원하는 확장 기능이다. 우와웅! 사실 안써봐서 좋은지는 몲.  
근데 이거 설치해서 쓸 바에 그냥 vscode 씀. 마소 짱짱맨
- [AutoDeriv](http://nodj.github.io/AutoDeriv/#install): **마켓에 없어서 수동 설치해야 함.** target이나 bin같은 소스가 컴파일되거나 배포되는 폴더는 이클립스 내에서 빠른 열기<kbd>ctrl + shift + r</kbd>의 대상에서 제외하는게 편하다. 제외하는 방법은 간단한데, 해당 폴더의 속성을 'Derived'로 설정하면 끝. (derived resource: 파생된 자원. 소스 코드가 아님을 의미) 문제는 폴더째로 삭제되고 다시 생성되었을 때 설정한 속성이 날라간다는 점이다. 이럴 때 필요한 확장 기능.  
이 확장 기능은 설치만 하면 끝나는게 아니라, workspace 아래에 `.derived` 파일을 만들고 아래처럼 내용을 작성해야 한다:
  ```bash
  # set the 'target' and 'ext' folders as derived
  target
  ext
  # but don't affect the 'keep' sub-folder
  !target/keep
  # all files with a '.dep' extension are generated
  *.dep
  src/include/version.h# this specific file is also generated.
  ```
- [Snyk Security Scanner](https://snyk.io/): 써드 파티 라이브러리의 취약점 등을 찾아주는(혹은 취약점이 보고된 라이브러리를 찾아주는) 확장 기능. 한 번 설치해서 돌려봤는데 보고서의 한글이 깨진다. ~~占쏙옙占쏙옙~~ node.js 버전이 메인인것 같으니 이걸 쓰자. 원래 이클립스 확장 기능은 뭐든지 션찮음.


## 작성자가 쓰는 단축키 설정

**사실 이런 뻘짓하지 말고 내보내기-불러오기 하는게 좋다.**

- Open Implementation: <kbd>f4</kbd>(In Windows, Navigate) Open Implementation은 Open Declaration(F3)과 다르게 인터페이스가 아니라 구체화된 클래스로 이동시킨다.
- Open Type Hierarchy: <kbd>unbined</kbd>
- Find Text In File: <kbd>ctrl + alt + f</kbd>(In Windows, Search)
- Watch: <kbd>ctrl + alt + w</kbd>(In Dialogs and Windows 혹은 Debugging Java) 디버깅 기능으로 특정 변수, 혹은 표현식을 감시한다.
- Compare with HEAD Revision: <kbd>ctrl + alt + home</kbd>(In Windows, Git)
- Compare with Previous Revision: <kbd>ctrl + alt + pageup</kbd>(In Windows, Git)
- Show in History: <kbd>ctrl + alt + pagedown</kbd>(In Windows, Git)
- ~~Show Key Assist: <kbd>ctrl + 0</kbd>(In Dialogs and Windows, Window)~~
- Add Bookmark: <kbd>ctrl + alt + z</kbd>
- Build Automatically: <kbd>ctrl + alt + insert</kbd>(In Dialogs and Windows, Project)
- Show Revision Information: <kbd>ctrl + alt + a</kbd>(In Windows, Git)
- Print: <kbd>unbined</kbd>
- ~~Quick Search: <kbd>ctrl + alt + l</kbd>(In Windows, Quick Search)~~
- Next Editor: <kbd>ctrl + 6</kbd>(In Windows, Window)
- Previous Editor: <kbd>ctrl + shift + 6</kbd>(In Windows, Window)
- Next View: <kbd>ctrl + 7</kbd>(In Windows, Window) 이 키 조합의 기존 기능인 Toggle Comment는 지워버릴것. 어차피 다른 단축키 두 개나 설정되어 있음.
- Previous View: <kbd>ctrl + shift + 7</kbd>(In Windows, Window)
- Next Perspective: <kbd>ctrl + 8</kbd>(In Windows, Window)
- Previous Perspective: <kbd>ctrl + shift + 8</kbd>(In Windows, Window)
- Push to Upstream: <kbd>ctrl + shift + p</kbd>(In Dialogs and Windows, Git)
- Pull: <kbd>ctrl + shift + l</kbd>(In Dialogs and Windows, Git)
- Fetch from Upstream: <kbd>ctrl + shift + f</kbd>(In Dialogs and Windows, Git)

Show History 같은 명령은 단축키가 작동하지 않을때가 있는데 이 때는 `Customize Perspective > Action Set Availability`에서 해당 범주를 추가해야 한다. (e.g. SVN의 show history 명령은 SVN을 추가)
