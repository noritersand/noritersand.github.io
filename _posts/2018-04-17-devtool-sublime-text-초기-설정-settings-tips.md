---
layout: post
date: 2018-04-17 13:32:10 +0900
title: '[devtool] Sublime Text 초기 설정과 팁'
categories:
  - devtool
tags:
  - devtool
  - sublimetext
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.sublimetext.com/docs/index.html](https://www.sublimetext.com/docs/index.html)
- [https://docs.sublimetext.io/guide/](https://docs.sublimetext.io/guide/)

#### 버전 정보

- Build 4xxx


## 개요

서브라임 텍스트 시리즈의 기본 설정, 단축키 등 정리.


## 기본 설정

### 터미널에서 서브라임 실행하기

Windows에선 아래 세 가지 방법이 있는데:

- 설치 폴더(기본 설정이면 `C:\Program Files\Sublime Text 3`)를 시스템 환경 변수 `Path`에 추가한다.
- 설치 폴더의 `subl.exe` 파일을 `C:\Windows\System32` 경로에 복사한다.
- [파워셸 프로파일](https://docs.microsoft.com/ko-kr/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.2)에 별칭을 추가한다.

세 번째 방법을 추천. 방법은 아래 코드를 복사해서 파워셸에 입력한다:

```js
if (!(Test-Path -Path $PROFILE)) {
  New-Item -ItemType File -Path $PROFILE -Force
}
"Set-Alias sb 'C:\Program Files\Sublime Text\subl.exe'" >> $PROFILE
. $PROFILE
```

이게 끝이고 이제 `sb`로 서브라임 실행 가능:

```bash
sb  # 서브라임 실행
sb .  # 새 서브라임을 실행하면서 현재 경로를 Open Folder로 열기
sb .\.gitignore  # .gitignore 파일을 서브라임으로 열기
```

WSL에선 셸 설정파일(rc)에 호스트 PC의 경로를 별칭으로 추가하면 끝:

```bash
alias sb='/mnt/c/Program\ Files/Sublime\ Text/subl.exe'
```

### 코드 스니펫 추가하기

서브라임에서 자동 완성 항목을 추가하는 방법이다.

메뉴에서 `Tools > Developer > New Snippet...` 을 누르면 새 스니펫 파일이 열린다. 거기에 아래처럼 작성한 뒤:

```xml
<!-- javascript-cl.sublime-snippet -->
<snippet>
  <content><![CDATA[
console.log(${1});
]]></content>
  <tabTrigger>cl</tabTrigger>
  <scope>source.js</scope>
</snippet>
```

```xml
<!-- javascript-cl2.sublime-snippet -->
<snippet>
  <content><![CDATA[
console.log('${1:msg}:', ${2:msg});
]]></content>
  <tabTrigger>cl2</tabTrigger>
  <scope>source.js</scope>
</snippet>
```

```xml
<!-- javascript-cd.sublime-snippet -->
<snippet>
  <content><![CDATA[
console.log(${1});
]]></content>
  <tabTrigger>cd</tabTrigger>
  <scope>source.js</scope>
</snippet>
```

```xml
<!-- javascript-cd2.sublime-snippet -->
<snippet>
  <content><![CDATA[
console.debug('${1:msg}:', ${2:msg});
]]></content>
  <tabTrigger>cd2</tabTrigger>
  <scope>source.js</scope>
</snippet>
```

확장자명을 반드시 `sublime-snippet`으로 해서 패키지 파일 디렉터리에 저장한다. 패키지 파일 디렉터리는 윈도우 기준 `%APPDATA%\Sublime Text\Packages\User`이며 저장할 때 자동으로 지정된다.

⚠️ **`<snippet>` 태그는 스니펫 파일의 루트 태그여야 한다. 그러니까 `<snippet>` 하나당 스니펫 파일 하나씩이다.**

작성한 파일을 다시 열어보고 싶으면 `View Package File` 명령을 실행할 것.


## 패키지 Sublime Text Packages

패키지는 서브라임 커뮤니티에 공유되는 플러그인 같은 거라고 생각하면 된다.

일단 package control을 설치한다. 커맨드 팔레트<kbd>ctrl + shift + p</kbd>에서 'install package control' 입력 후 엔터.

설치가 끝나면 (<kbd>ctrl + \`</kbd> 눌러서 확인 가능) 다시 커맨드 팔레트에서 'Package Control: Install Package' 입력하면 패키지 검색 창이 뜬다. 여기서 원하는 패키지 검색 후 엔터 누르면 됨.

### 추천 패키지

#### [⭐ MoveTab](https://packagecontrol.io/packages/MoveTab)

<kbd>ctrl + shift + pageup/pagedown</kbd>으로 탭의 위치를 좌우로 이동한다.

#### [StyleToken](https://packagecontrol.io/packages/StyleToken)

파일 내에서 특정 단어별 하이라이팅

#### [FileDiffs](https://packagecontrol.io/packages/FileDiffs)

간단한 diff 뷰어. diff 성능 자체는 그닥... (shell의 기본 diff와 거의 비슷)

#### [ConvertToUTF8](https://packagecontrol.io/packages/ConvertToUTF8)

`EUC-KR`로 작성된 파일을 `UTF-8`로 전환해서 열어주는 패키지. 이 패키지를 활성화하면 파일을 열때마다 인코딩을 물어봐서 좀 귀찮음

#### [⭐ BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter)

브라켓 하이라이터. 괄호가 어디서 시작하고 어디서 끝나는지 행번호 표시영역에 아이콘으로 표시해준다.

#### [Sync View Scroll](https://packagecontrol.io/packages/Sync%20View%20Scroll)

여러 view의 스크롤을 동기화하는 패키지. 심지어 좌우 스크롤도 동기화된다.

#### [URLEncode](https://packagecontrol.io/packages/URLEncode)

URL 인코드-디코드 기능 제공.

#### [HexViewer](https://packagecontrol.io/packages/HexViewer)

주기능은 HEX 파일 뷰어, 부기능으로 HEX-텍스트간 변환과 해시 생성 등을 지원하는 패키지. 좌측에 HEX, 우측에 일반 텍스트를 동시에 표시해줘서 포커스된 문자를 하이라이팅 해주는 등 뷰어 기능이 쓸만함.

#### [⭐ SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements)

서브라임의 부실한 사이드바(파일 탐색기) 기능을 보충해주는 패키지.

#### [⭐ MarkdownPreview](https://packagecontrol.io/packages/MarkdownPreview)

마크다운 파일 브라우저로 미리보기

#### [⭐ Insert Nums](https://packagecontrol.io/packages/Insert%20Nums)

늘어난 캐럿만큼 순번을 자동으로 입력해줌. 시작 번호와 증가치를 지정할 수 있음. 기본 단축키는 <kbd>ctrl + alt + n</kbd>, <kbd>ctrl + alt + shift + n</kbd>

#### [⭐ Emmet](https://packagecontrol.io/packages/Emmet)

예전 이름은 Zen coding이었던 축약어로 마크업을 완성해주는 Emmet 지원 패키지. Emmet 문법은 [여기](https://docs.emmet.io/)를 보면 됨.

#### [Log Highlight](https://packagecontrol.io/packages/Log%20Highlight)

로그 파일 가독성이 아주 약간 좋아짐.

#### ⛔ ~~[Compare Side-By-Side](https://packagecontrol.io/packages/Compare%20Side-By-Side)~~

~~FileDiffs보다 보기 좋은 diff 뷰어. 단축키는 alt + n(다음), alt + p(이전)~~

이 패키지는 기본 단축키를 바꿔버려서 안쓰기로 함.

#### [⭐ Clickable URLs](https://packagecontrol.io/packages/Clickable%20URLs)

URL에 해당하는 텍스트에 커서를 놓고(혹은 드래그 후) 단축키 <kbd>ctrl + alt + enter</kbd>를 누르면 브라우저로 연결함.  
**⚠️ 설치하면 기본 단축키인 `replace_all`을 덮어쓰니 주의**

#### [⭐ Case Conversion](https://packagecontrol.io/packages/Case%20Conversion)

영단어 케이스 변환 기능 제공. 사용 방법은 커맨트 팔레트에서 'case convert' 치면 주르륵 나옴.  

두문자어를 무시('userID'를 'userId'로 변환)하고 싶은 경우 `Preferences > Package Settings > Case Conversion > Settings`로 진입한 뒤 이걸 붙여넣으면 된다:

```
{"detect_acronyms": false}
```

#### [🌟 InsertDate](https://github.com/FichteFoll/InsertDate)

2015년이 마지막 커밋이지만 서브라임4에서도 잘 작동하는 날짜 + 시간 입력기. 기본 단축키는 <kbd>f5</kbd>이며 [strftime](https://www.strfti.me) 포맷 커스텀 입력은 <kbd>alt + f5</kbd>.

#### [⭐ Pretty JSON](https://packagecontrol.io/packages/Pretty%20JSON)

JSON 문자열을 한 줄로 압축하거나 반대로 예쁘게 포맷해주는 플러그인


## 작성자 저장용 사용자 설정

### settings - user

```json
{
  "fallback_encoding": "UTF-8",
  "font_face": "Consolas",
  "font_size": 10,
  "tab_completion": false,
  "auto_complete": true,
  "auto_complete_commit_on_tab": true,
  "auto_complete_with_fields": true,
  "show_encoding": true,
  "show_line_endings": true,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "save_on_focus_lost": true,
  "show_full_path": false,
  "show_rel_path": true,
  "open_tabs_after_current": false
}
```

### key bindings - user

```json
[
  { "keys": ["f1"], "command": "show_overlay", "args": {"overlay": "command_palette"} },
  { "keys": ["ctrl+shift+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },
  { "keys": ["ctrl+shift+k"], "command": "duplicate_line" },
  { "keys": ["ctrl+shift+s"], "command": "save_all" },
  { "keys": ["ctrl+k", "ctrl+k"], "command": "do_nothing" },
  { "keys": ["ctrl+k", "ctrl+backspace"], "command": "do_nothing" },
  { "keys": ["ctrl+b"], "command": "do_nothing" }
]
```

걸리적거려서 기본 단축키 몇 개는 끔.


## 기본 단축키

Build 3126 이후에 기록함.

무슨 명령인지 모르겠으면 [여기](https://docs.sublimetext.io/reference/commands.html#about-paths-in-command-arguments)를 보자.

### 단어 선택

- <kbd>ctrl + shift + right/left</kbd>: 단어(words) 단위의 선택 영역을 좌우로 확장한다. 단어란 언더바`_`를 포함한 연속적인 단어 구성 문자를 의미한다. (abc, abcDef, ABC_DEF, ...)
- <kbd>alt + shift + right/left</kbd>: 보조 단어(subwords) 단위의 선택 영역을 확장한다. 단어와 다르게 보조 단어는 앞단어와 다른 대소문자와 모든 특수문자로 구분된다.

### 멀티 캐럿(Multiple Selection)

서브라임에선 Expand Selection 기능으로 분류한다.

캐럿을 추가해 여러 지역에서 작업을 동시에 할 때 사용하는 기능. 에디터마다 부르는 이름이 제각각이다. 서브라임은 Expand Selection, 아톰과 VSCODE는 Add Selection, 인텔리제이는 Select Next Occurrence 😒

- <kbd>ctrl + d</kbd>: 선택한 단어와 동일한 다음 단어에 캐럿 추가
- <kbd>ctrl + u</kbd>: 캐럿 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: select_lines 위나 아래로 멀티 캐럿
- <kbd>alt + f3</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 캐럿
- <kbd>ctrl + shift + l</kbd>: split_selection_into_lines 선택한 영역에서 각 라인마다 캐럿 분리

### 커서/포커스 이동

- <kbd>alt + \-</kbd>: Jump Back 이전 포커스로 이동. 이클립스의 <kbd>alt + ←</kbd>와 비슷
- <kbd>alt + shift +  '+'</kbd>: Jump Forward 다음 포커스로 이동. 이클립스의 <kbd>alt + →</kbd>와 비슷
- <kbd>ctrl + r</kbd>: 현재 파일의 모든 심볼(함수, 변수, 프로퍼티, 제목 등) 보기. 선택하면 포커스 이동.
- <kbd>ctrl + shift + r</kbd>: 현재 프로젝트(혹은 열려있는 폴더)의 모든 심볼 보기. 선택하면 포커스 이동.
- <kbd>f12</kbd>: Goto Definition 현재 커서가 있는 함수나 메서드의 선언부로 이동. 제한적으로 작동하는 기능(Syntax 보기 형태와 문서의 내용이 알맞아야 함)이다.
- <kbd>shift + f12</kbd>: Goto Reference 함수나 메서드를 사용(참조)하고 있는 라인으로 이동.
- <kbd>ctrl + 0</kbd>: 사이드바로 포커스 이동
- <kbd>alt + 숫자키</kbd>: 열려진 탭 사이의 포커스 이동
- <kbd>alt + shift + 숫자키</kbd>: 레이아웃 나누기
- <kbd>ctrl + 숫자키</kbd>: 레이아웃이 나눠진 상태에서 다른 레이아웃으로 포커스 이동
- <kbd>ctrl + r</kbd>: Goto Symbol
- <kbd>ctrl + shift + r</kbd>: Goto Project Symbol

### Code Folding

특정 코드 영역을 접고 펴는 기능

- <kbd>ctrl + shift + [</kbd>: Fold (보통은 괄호 등으로 작성된 코드 블록을) 접기
- <kbd>ctrl + shift + ]</kbd>: Unfold 펼치기
- <kbd>ctrl + k, ctrl + 1</kbd>: Fold All 모두 접기
- <kbd>ctrl + k, ctrl + j</kbd>: Unfold All 접혀있는 코드 전부 펼치기
- <kbd>ctrl + k, ctrl + 숫자키</kbd>: Fold Level 2~9 코드 깊이에 따라 해당하는 모든 코드를 접는 기능이다. 숫자키는 2부터 9까지 가능

### 편집

- <kbd>ctrl + shift + j</kbd>: 라인 단위 병합
- <kbd>F9</kbd>: 대소문자 무시하고 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + F9</kbd>: 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + m</kbd>: 가까운 닫는 괄호(bracket) 혹은 여는 괄호로 이동.
- <kbd>ctrl + shift + m</kbd>: 가까운 닫는 괄호까지 텍스트 선택.

### 오버레이 Overlay

- <kbd>ctrl + \`</kbd>: 콘솔창
- <kbd>ctrl + shift + p</kbd>: Command Palatte 빠른 명령어 탐색창 열기
- <kbd>ctrl + p</kbd>: (폴더 열기 이후)빠른 파일 탐색창 열기
- <kbd>ctrl + r</kbd>: 함수 단위 탐색창 열기
- <kbd>ctrl + g</kbd>: 라인 이동
- <kbd>ctrl + ;</kbd>: 키워드 탐색창 열기
- <kbd>ctrl + alt + shift + p</kbd>: Show Scope Name 스코프 이름 보기. 현재 캐럿이 위치한 곳 기준으로 스코프 정보를 툴팁으로 표시하는 기능이다. 그런데 소스 코드의 스코프가 아니라, 서브라임 텍스트의 환경 기준의 스코프를 의미한다. 그러니께 서브라임 패키지 개발자용 기능

### 매크로

- <kbd>ctrl + q</kbd>: 매크로 기록 시작/종료
- <kbd>ctrl + shift + q</kbd>: 매크로 실행 (하나밖에 안 되나 보다)

### 기타 조합형 단축키

- <kbd>ctrl + k</kbd>: 단축키 시퀀스 시작
- <kbd>ctrl + k, ctrl + u</kbd>: 대문자 변환
- <kbd>ctrl + k, ctrl + l</kbd>: 소문자 변환
- <kbd>ctrl + k, ctrl + b</kbd>: 사이드 바 토글. 서브라임 머지에도 같은 단축키로 작동함.

### 북마크 Bookmark

활성화된 파일 내에서만 작동하는 북마크 기능

- <kbd>ctrl + f2</kbd>: 현재 캐럿 위치에 북마크 생성
- <kbd>f2</kbd>: 다음 북마크로 이동
- <kbd>shift + f2</kbd>: 이전 북마크로 이동
- <kbd>ctrl + shift + f2</kbd>: 현재 파일의 북마크를 모두 삭제한다.
- <kbd>alt + f2</kbd>: 모든 북마크 위치에 캐럿을 생성한다.

### 마크 Mark

현재 캐럿이 위치한 라인과 컬럼을 마킹하는 기능. 마킹지점을 기준으로 범위 선택이나 범위 삭제를 할 수 있다.

- <kbd>ctrl + k, ctrl + space</kbd>: 마크 만들기
- <kbd>ctrl + k, ctrl + a</kbd>: 현재 커서의 위치부터 마크까지 선택(드래그 블록)
- <kbd>ctrl + k, ctrl + w</kbd>: 현재 커서의 위치부터 마크까지 삭제
- <kbd>ctrl + k, ctrl + x</kbd>: 현재 커서의 위치에 새로 마크를 만들고, 이미 마킹되어 있던 지점으로 커서 이동
- <kbd>ctrl + k, ctrl + g</kbd>: 마크 삭제
- <kbd>ctrl + k, ctrl + y</kbd>: Yank 기능이라는데 어떻게 작동하는 건지 몲.

### 프로젝트, 워크스페이스

- <kbd>alt + shift + p</kbd>: Quick Switch Project 빠른 프로젝트 변경 창 열기
