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

ctrl+shift+p 로 팔레트를 열고 install package control

## 한글 인코딩 지원 패키지 설치

`ctrl + shift + p`로 보이는 커맨드 창에서 'install package' 입력 후 엔터, `ConvertToUTF8` 선택

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

#### StyleToken

[https://packagecontrol.io/packages/StyleToken](https://packagecontrol.io/packages/StyleToken)

파일 내에서 특정 단어별 하이라이팅 기능.

#### File​Diffs

[https://packagecontrol.io/packages/FileDiffs](https://packagecontrol.io/packages/FileDiffs)

간단한 diff 뷰어. diff 성능 자체는 그닥... (shell의 기본 diff와 거의 비슷)

#### Sublimerge

[http://www.sublimerge.com/](http://www.sublimerge.com/)

diff 기능이 매우 좋은 플러그인이지만 유료다. (35달러)

#### ConvertToUTF8

[https://packagecontrol.io/packages/ConvertToUTF8](https://packagecontrol.io/packages/ConvertToUTF8)

서브라임 텍스트가 지원하지 않는 인코딩으로 저장된 파일을 저장하거나 수정할 수 있게 한다. (가령 EUC-KR 같은)

#### WinMerge

[https://packagecontrol.io/packages/WinMerge](https://packagecontrol.io/packages/WinMerge)

서브라임에서 마지막으로 활성화한 view와 현재 view를 WinMerge를 실행해 비교하는 플러그인. 물론 WinMerge가 깔려있어야 한다.

#### Bracket​Highlighter

[https://packagecontrol.io/packages/BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter)

괄호의 시작과 끝을 행 번호 영역에 표시해주는 플러그인.

#### Compare Side-By-Side

[https://packagecontrol.io/packages/Compare%20Side-By-Side](https://packagecontrol.io/packages/Compare%20Side-By-Side)

FileDiffs보다 보기 좋은 diff 뷰어.

단축키는:

- Jump to next: `alt + n`
- Jump to previous: `alt + p`

#### Sync View Scroll

[https://packagecontrol.io/packages/Sync%20View%20Scroll](https://packagecontrol.io/packages/Sync%20View%20Scroll)

여러 view의 스크롤을 동기화하는 플러그인. 심지어 좌우 스크롤도 동기화된다.

#### URLEncode

[https://packagecontrol.io/packages/URLEncode](https://packagecontrol.io/packages/URLEncode)

URL 인코드-디코드 기능 제공.

#### Case Conversion

[https://packagecontrol.io/packages/Case%20Conversion](https://packagecontrol.io/packages/Case%20Conversion)

영단어 케이스 변환 기능 제공. 사용 방법은 커맨트 팔레트에서 'case convert' 치면 주르륵 나옴.

두문자어를 무시('userID'를 'userId'로 변환)하고 싶은 경우 `Preferences` > `Package Settings` > `Case Conversion` > `Settings`로 진입한 뒤 아래를 붙여넣으면 된다:

```json
{ "detect_acronyms": false }
```

#### HexViewer

[https://packagecontrol.io/packages/HexViewer](https://packagecontrol.io/packages/HexViewer)

주기능은 HEX 파일 뷰어, 부기능으로 HEX-텍스트간 변환과 해시 생성 등을 지원하는 플러그인. 좌측에 HEX, 우측에 일반 텍스트를 동시에 표시해줘서 포커스된 문자를 하이라이팅 해주는 등 뷰어가 아주 훌륭함.
