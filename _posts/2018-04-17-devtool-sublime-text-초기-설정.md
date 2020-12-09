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

## package control 설치

커멘드 팔레트<kbd>ctrl + shift + p</kbd>에서 'install package control' 입력 후 엔터

## 한글 인코딩 지원 패키지 설치

정확히는 유니코드 문자가 포함되어 있으면서 UTF-8이 아닌 파일을 UTF-8로 변환하는 패키지다.  
커멘드 팔레트<kbd>ctrl + shift + p</kbd>에서 'install package' 입력 후 엔터, `ConvertToUTF8` 선택

## 작성자 저장용 사용자 설정

#### settings - user

```json
{
  "auto_complete": false,
  "fallback_encoding": "UTF-8",
  "font_face": "Consolas",
  "font_size": 11,
  "show_encoding": true,
  "show_line_endings": true,
  "tab_completion": false,
  "file_exclude_patterns": ["*.sublime-workspace"]
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

## 추천 패키지(플러그인)

#### [StyleToken](https://packagecontrol.io/packages/StyleToken)

파일 내에서 특정 단어별 하이라이팅 기능.

#### [File​Diffs](https://packagecontrol.io/packages/FileDiffs)

간단한 diff 뷰어. diff 성능 자체는 그닥... (shell의 기본 diff와 거의 비슷)

#### [Sublimerge](http://www.sublimerge.com/)

diff 기능이 매우 좋은 패키지. 하지만 유료. (35달러)

#### [ConvertToUTF8](https://packagecontrol.io/packages/ConvertToUTF8)

서브라임 텍스트가 기본 지원하지 않는 인코딩으로 저장된 파일을 저장하거나 수정할 수 있게 한다. (가령 EUC-KR 같은)

#### [WinMerge](https://packagecontrol.io/packages/WinMerge)

서브라임에서 마지막으로 활성화한 view와 현재 view를 WinMerge를 실행해 비교하는 패키지. 물론 WinMerge가 깔려있어야 한다.

#### [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter)

브라켓 하이라이터. 괄호가 어디서 시작하고 어디서 끝나는지 행번호 표시영역에 아이콘으로 표시해준다.

#### [Compare Side-By-Side](https://packagecontrol.io/packages/Compare%20Side-By-Side)

FileDiffs보다 보기 좋은 diff 뷰어.  
단축키는:  

- Jump to next: <kbd>alt + n</kbd>
- Jump to previous: <kbd>alt + p</kbd>

#### [Sync View Scroll](https://packagecontrol.io/packages/Sync%20View%20Scroll)

여러 view의 스크롤을 동기화하는 패키지. 심지어 좌우 스크롤도 동기화된다.

#### [URLEncode](https://packagecontrol.io/packages/URLEncode)

URL 인코드-디코드 기능 제공.

#### [Case Conversion](https://packagecontrol.io/packages/Case%20Conversion)

영단어 케이스 변환 기능 제공. 사용 방법은 커맨트 팔레트에서 'case convert' 치면 주르륵 나옴.  
두문자어를 무시('userID'를 'userId'로 변환)하고 싶은 경우 `Preferences` > `Package Settings` > `Case Conversion` > `Settings`로 진입한 뒤 아래를 붙여넣으면 된다:

```json
{ "detect_acronyms": false }
```

#### [HexViewer](https://packagecontrol.io/packages/HexViewer)

주기능은 HEX 파일 뷰어, 부기능으로 HEX-텍스트간 변환과 해시 생성 등을 지원하는 패키지. 좌측에 HEX, 우측에 일반 텍스트를 동시에 표시해줘서 포커스된 문자를 하이라이팅 해주는 등 뷰어가 아주 훌륭함.

## 기본 단축키 메모

Build 3126 이후에 기록함.

### 커서/포커스 이동

- <kbd>alt + -</kbd>: Jump Back. 이전 포커스로 이동. 이클립스의 <kbd>alt + ←</kbd>와 비슷
- <kbd>alt + shift +  '+'</kbd>: Jump Forward. 다음 포커스로 이동. 이클립스의 <kbd>alt + →</kbd>와 비슷
- <kbd>f12</kbd>: Goto Definition. 현재 커서가 있는 함수나 메서드의 선언부로 이동. 제한적으로 작동하는 기능(Syntax 보기 형태와 문서의 내용이 알맞아야 함)이다.
- <kbd>shift + f12</kbd>: Goto Reference. 함수나 메서드를 사용(참조)하고 있는 라인으로 이동.
- <kbd>ctrl + 0</kbd>: 사이드 바로 포커스 이동

### 편집

- <kbd>ctrl + j</kbd>: 라인 단위 병합
- <kbd>F9</kbd>: 대소문자 무시하고 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + F9</kbd>: 라인 단위 알파벳 오름차순 정렬
- <kbd>ctrl + m</kbd>: 괄호
- <kbd>ctrl + shift + m</kbd>: 괄호

### 멀티 커서

- <kbd>ctrl + d</kbd>: 선택한 단어 기준 멀티 커서
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 커서
- <kbd>alt + f3</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 커서

### overlay

- <kbd>ctrl + \`</kbd>: 콘솔창
- <kbd>ctrl + shift + p</kbd>: Command Palatte 빠른 명령어 탐색창 열기
- <kbd>ctrl + p</kbd>: (폴더 열기 이후)빠른 파일 탐색창 열기
- <kbd>ctrl + r</kbd>: 함수 단위 탐색창 열기
- <kbd>ctrl + g</kbd>: 라인 이동
- <kbd>ctrl + ;</kbd>: 키워드 탐색창 열기

### 조합형 단축키

- <kbd>ctrl + k</kbd>: 단축키 시퀀스 시작
- <kbd>ctrl + k</kbd>, <kbd>ctrl + u</kbd>: 대문자 변환
- <kbd>ctrl + k</kbd>, <kbd>ctrl + l</kbd>: 소문자 변환
- <kbd>ctrl + k</kbd>, <kbd>ctrl + b</kbd>: 사이드 바 토글

---

- <kbd></kbd>:
