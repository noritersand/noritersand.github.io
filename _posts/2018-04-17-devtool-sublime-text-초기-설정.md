---
layout: post
date: 2018-04-17 13:32:10 +0900
title: '[devtool] sublime text 초기 설정'
categories:
  - devtool
tags:
  - devtool
  - sublimetext
---

* Kramdown table of contents
{:toc .toc}

#### 버전 정보

- Sublime Text 4 (Build 4xxx)

## 개요

서브라임 텍스트 시리즈의 기본 설정, 단축키 등 메모.

## 기본 설정

### 터미널에서 서브라임 실행하기

아래 세 가지 방법이 있는데:

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

### 추가 패키지 설치

패키지는 서브라임 커뮤니티에서 만드는 사용자 플러그인 같은 거라고 생각하면 된다.

일단 package control을 설치한다. 커맨드 팔레트<kbd>ctrl + shift + p</kbd>에서 'install package control' 입력 후 엔터.

설치가 끝나면 (<kbd>ctrl + \`</kbd> 눌러서 확인 가능) 다시 커맨드 팔레트에서 'Package Control: Install Package' 입력하면 패키지 검색 창이 뜬다. 여기서 원하는 패키지 검색 후 엔터 누르면 됨.

#### 추천 패키지

- [MoveTab](https://packagecontrol.io/packages/MoveTab): <kbd>ctrl + shift + pageup/pagedown</kbd>으로 탭의 위치를 좌우로 이동한다.
- [StyleToken](https://packagecontrol.io/packages/StyleToken): 파일 내에서 특정 단어별 하이라이팅 기능.
- [File​Diffs](https://packagecontrol.io/packages/FileDiffs): 간단한 diff 뷰어. diff 성능 자체는 그닥... (shell의 기본 diff와 거의 비슷)
- [ConvertToUTF8](https://packagecontrol.io/packages/ConvertToUTF8): 서브라임에서 기본적으로 지원하지 않는 인코딩, 가령 EUC-KR 등을 지원하게 해주는 패키지.
- [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter): 브라켓 하이라이터. 괄호가 어디서 시작하고 어디서 끝나는지 행번호 표시영역에 아이콘으로 표시해준다.
- ~~[Compare Side-By-Side](https://packagecontrol.io/packages/Compare%20Side-By-Side): FileDiffs보다 보기 좋은 diff 뷰어. 단축키는 <kbd>alt + n</kbd>(다음), <kbd>alt + p</kbd>(이전)~~ 근데 기본 단축키를 바꿔버려서 안쓰기로 함.
- [Sync View Scroll](https://packagecontrol.io/packages/Sync%20View%20Scroll): 여러 view의 스크롤을 동기화하는 패키지. 심지어 좌우 스크롤도 동기화된다.
- [URLEncode](https://packagecontrol.io/packages/URLEncode): URL 인코드-디코드 기능 제공.
- [HexViewer](https://packagecontrol.io/packages/HexViewer): 주기능은 HEX 파일 뷰어, 부기능으로 HEX-텍스트간 변환과 해시 생성 등을 지원하는 패키지. 좌측에 HEX, 우측에 일반 텍스트를 동시에 표시해줘서 포커스된 문자를 하이라이팅 해주는 등 뷰어 기능이 쓸만함.
- [Clickable URLs](https://packagecontrol.io/packages/Clickable%20URLs): URL에 해당하는 텍스트에 커서를 놓고(혹은 드래그 후) 단축키 <kbd>ctrl + alt + enter</kbd>를 누르면 브라우저로 연결함.
- [Case Conversion](https://packagecontrol.io/packages/Case%20Conversion): 영단어 케이스 변환 기능 제공. 사용 방법은 커맨트 팔레트에서 'case convert' 치면 주르륵 나옴.  
  두문자어를 무시('userID'를 'userId'로 변환)하고 싶은 경우 `Preferences > Package Settings > Case Conversion > Settings`로 진입한 뒤 이걸 붙여넣으면 된다:
  ```
  { "detect_acronyms": false }
  ```
- [SideBarEnhancements](https://packagecontrol.io/packages/SideBarEnhancements): 서브라임의 부실한 사이드바(파일 탐색기) 기능을 보충해주는 패키지.

## 작성자 저장용 사용자 설정

### Sublime Text

#### settings - user

```json
{
  "fallback_encoding": "UTF-8",
  "font_face": "Consolas",
  "font_size": 11,
  "tab_completion": false,
  "auto_complete": true,
  "auto_complete_commit_on_tab": true,
  "auto_complete_with_fields": true,
  "show_encoding": true,
  "show_line_endings": true,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "save_on_focus_lost": true
}
```

#### key bindings - user

```json
[
  { "keys": ["ctrl+shift+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },
  { "keys": ["ctrl+shift+k"], "command": "duplicate_line" },
  { "keys": ["ctrl+k", "ctrl+k"], "command": "do_nothing" },
  { "keys": ["ctrl+k", "ctrl+backspace"], "command": "do_nothing" },
  { "keys": ["ctrl+shift+s"], "command": "save_all" },
  { "keys": ["f1"], "command": "show_overlay", "args": {"overlay": "command_palette"} }
]
```

### 번외: Sublime Merge

#### key bindings - user

현재(2022-05-04) 공식 문서에서 command 목록을 찾을 수가 없다. 그래서 [답답한 누군가가 만들어놓은걸](https://github.com/Sublime-Instincts/CommandsBrowser/tree/master/sm_commands_metadata)로 확인해야 함.

```json
[
  { "keys": ["f1"], "command": "show_command_palette" },
  { "keys": ["ctrl+p"], "command": "quick_switch_repository" },
  { "keys": ["ctrl+shift+alt+a"], "command": "stage_all" },
  { "keys": ["ctrl+shift+alt+u"], "command": "unstage_all" },
  { "keys": ["ctrl+,"], "command": "open_preferences" }
]
```

`stage_all`은 untracked 파일도 같이 스테이징하는 명령이다. 모든 변경사항 취소하는 것도 있는데 `{ "keys": ["ctrl+shift+alt+d"], "command": "discard_all_modified" }` 위험해서 빼앰.

## 기본 단축키 메모

Build 3126 이후에 기록함.

### 커서/포커스 이동

- <kbd>alt + -</kbd>: Jump Back. 이전 포커스로 이동. 이클립스의 <kbd>alt + ←</kbd>와 비슷
- <kbd>alt + shift +  '+'</kbd>: Jump Forward. 다음 포커스로 이동. 이클립스의 <kbd>alt + →</kbd>와 비슷
- <kbd>ctrl + r</kbd>: 현재 파일의 모든 심볼(함수, 변수, 프로퍼티, 제목 등) 보기. 선택하면 포커스 이동.
- <kbd>ctrl + shift + r</kbd>: 현재 프로젝트(혹은 열려있는 폴더)의 모든 심볼 보기. 선택하면 포커스 이동.
- <kbd>f12</kbd>: Goto Definition. 현재 커서가 있는 함수나 메서드의 선언부로 이동. 제한적으로 작동하는 기능(Syntax 보기 형태와 문서의 내용이 알맞아야 함)이다.
- <kbd>shift + f12</kbd>: Goto Reference. 함수나 메서드를 사용(참조)하고 있는 라인으로 이동.
- <kbd>ctrl + 0</kbd>: 사이드바로 포커스 이동
- <kbd>alt + 숫자키</kbd>: 열려진 탭 사이의 포커스 이동
- <kbd>alt + shift + 숫자키</kbd>: 레이아웃 나누기
- <kbd>ctrl + 숫자키</kbd>: 레이아웃이 나눠진 상태에서 다른 레이아웃으로 포커스 이동
- <kbd>ctrl + r</kbd>: Goto Symbol
- <kbd>ctrl + shift + r</kbd>: Goto Project Symbol

### 멀티 캐럿

Expand Selection

캐럿을 추가해 여러 지역에서 작업을 동시에 할 때 사용하는 기능. 에디터마다 부르는 이름이 제각각이다. 서브라임은 Expand Selection, 아톰과 VSCODE는 Add Selection, 인텔리제이는 Select Next Occurrence 😒

- <kbd>ctrl + d</kbd>: 선택한 단어와 동일한 다음 단어에 캐럿 추가
- <kbd>ctrl + u</kbd>: 캐럿 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 캐럿
- <kbd>alt + f3</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 캐럿
- <kbd>ctrl + shift + l</kbd>: split selection into lines, 선택한 영역에서 각 라인마다 캐럿 분리

### 편집

- <kbd>ctrl + shift + j</kbd>: 라인 단위 병합
- <kbd>F9</kbd>: 대소문자 무시하고 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + F9</kbd>: 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + m</kbd>: 가까운 닫는 괄호(bracket) 혹은 여는 괄호로 이동.
- <kbd>ctrl + shift + m</kbd>: 가까운 닫는 괄호까지 텍스트 선택.

### overlay

- <kbd>ctrl + \`</kbd>: 콘솔창
- <kbd>ctrl + shift + p</kbd>: Command Palatte 빠른 명령어 탐색창 열기
- <kbd>ctrl + p</kbd>: (폴더 열기 이후)빠른 파일 탐색창 열기
- <kbd>ctrl + r</kbd>: 함수 단위 탐색창 열기
- <kbd>ctrl + g</kbd>: 라인 이동
- <kbd>ctrl + ;</kbd>: 키워드 탐색창 열기

### 조합형 단축키

- <kbd>ctrl + k</kbd>: 단축키 시퀀스 시작
- <kbd>ctrl + k, ctrl + u</kbd>: 대문자 변환
- <kbd>ctrl + k, ctrl + l</kbd>: 소문자 변환
- <kbd>ctrl + k, ctrl + b</kbd>: 사이드 바 토글
