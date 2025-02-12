---
layout: post
date: 2018-06-19 11:12:54 +0900
title: '[devtool] Atom 초기 설정 및 팁'
categories:
  - devtool
tags:
  - devtool
  - atom
---

* Kramdown table of contents
{:toc .toc}

**😱 [2022년 12월 15일까지 모든 프로젝트를 중단한다고 발표함](https://github.blog/2022-06-08-sunsetting-atom/)**

## 기본 설정

### 맞춤법 검사 비활성화

`Settings > Installed Packages > spell-check`에서 'disable' 클릭

### 자동 완성 설정 변경

`Settings > Installed Packages > autocomplete-plus`의 설정에서:

- `Show Suggestions On Keystroke`이 체크된 상태면 타이핑 할 때마다 자동 완성 목록이 나타남. 취향대로 설정할 것.
- `Keymap For Confirming A Suggestions`을 `tab`으로 변경해서 엔터에는 반응하지 않도록 함.
- `Use Core Movement Commands`는 체크되어 있는게 편함.

참고: 자동 완성 단축키: <kbd>ctrl + space</kbd>

### 검색창 설정

`Settings > Installed Packages > find-and-replace`의 설정에서:

- 밑에 내려서 `Scroll To Result On Live-Search` 체크. 요게 incremental 검색 옵션임.

### 마크다운 미리보기 설정 변경

`Settings > Installed Packages > markdown-preview`의 설정에서:

- `Live Update` 체크 해제: 글이 길 때 미리보기가 자동갱신되면 렉이 심함.
- `Open Preview In Split Pane` 체크 해제: 새 탭으로 열리는게 더 편함.

### 자동 저장

`Settings > Installed Packages > autosave`의 설정에서 'Enabled' 체크.

### 자동 저장

`Settings > Installed Packages > fuzzy-finder`의 설정에서 `Ignored Names` 항목에 file finder(<kbd>ctrl + p</kbd>)가 무시할 패턴 입력.

### 붙여넣기 시 자동 들여쓰기 해제

`Settings > Editor`에서 `Auto Indent On Paste` 체크 해제.

### 개행, 탭, 공백 문자 표시

`Settings > Editor`에서 `Show Invisibles` 체크.

### 탭 타입 변경

`Settings > Editor`에서 'Tab Type'을 'soft'로 변경. 이 옵션은 탭 문자를 스페이스바로 변경함을 의미한다.

## 작성자 저장용 단축키 설정

커맨드 팔레트(<kbd>ctrl + shift + p</kbd>)에서 'Application: Open Your Keymap' 입력 후 엔터. 그리고 열리는 keymap.cson을 아래처럼 변경:

```yml
'.platform-win32, .platform-win32 .command-palette atom-text-editor':
  'f1': 'command-palette:toggle' # f1으로도 명령창 열기

'atom-workspace':
  'ctrl-i': 'nothing' # goto-last-edit 패키지 키 변경
  'ctrl-alt-i': 'nothing' # goto-last-edit 패키지 키 변경
  'alt--': 'goto-last-edit:back' # goto-last-edit 패키지 키 변경. 서브라임하고 맞춤
  'alt-shift--': 'goto-last-edit:forward' # goto-last-edit 패키지 키 변경. 서브라임하고 맞춤

'atom-workspace atom-text-editor:not([mini])':
  'ctrl-shift-k': 'editor:duplicate-lines'
  'ctrl-j': 'nothing'
  'ctrl-shift-j': 'editor:join-lines'

'atom-text-editor:not([mini])':
  'ctrl-shift-d': 'editor:delete-line' # 라인 삭제
  'ctrl-shift-up': 'editor:move-line-up' # 현재 라인 위로 이동
  'ctrl-shift-down': 'editor:move-line-down' # 현재 라인 아래로 이동
  'f5': 'nothing'
  'f9': 'sort-lines:case-insensitive-sort' # sort-lines 패키지 키 변경. f9로 대소문자 구분 없이 정렬
  'ctrl-f9': 'sort-lines:sort' # sort-lines 패키지 키 변경. 대소문자 구분하는 정렬

'atom-text-editor':
  # 'ctrl-up': 'keyboard-scroll:scrollUp'
  # 'ctrl-down': 'keyboard-scroll:scrollDown'
  'ctrl-up': 'nothing'
  'ctrl-down': 'nothing'
  'alt-up': 'nothing'
  'alt-down': 'nothing'
  'ctrl-shift-l': 'editor:split-selections-into-lines' # 여러 라인을 선택한 상태에서 각 라인에 커서 생성. 서브라임하고 맞춤

'body':
  'ctrl-shift-pageup': 'pane:move-item-left' # 활성화된 탭을 좌우로 이동
  'ctrl-shift-pagedown': 'pane:move-item-right' # 활성화된 탭을 좌우로 이동
```

## 추천 패키지(플러그인)

- ~~[keyboard-scroll](https://atom.io/packages/keyboard-scroll)~~: ~~다른 에디터처럼 키보드로 스크롤만 한 줄씩 이동할 때 필요함.~~  
  멀티 캐럿 증식(?)키(<kbd>ctrl + alt + up/down</kbd>)가 이상작동하는 현상이 있음.
- [url-encode](https://atom.io/packages/url-encode): ?
- [goto-last-edit](https://atom.io/packages/goto-last-edit): <kbd>ctrl + i</kbd> 혹은 <kbd>ctrl + alt + i</kbd>로 마지막 수정 이력 이동... 인데 서브라임이랑 같도록 바꿔서 씀. 키 바꿔서 쓸꺼면 해당 패키지 settings에서 `Keybindings`는 끌 것
- [sort-lines](https://atom.io/packages/sort-lines): 아톰에 없는 sorting 기능 추가. 기본 단축키는 <kbd>f5</kbd>지만 서브라임이랑 키 같게 수정함.
- [sync-settings](https://atom.io/packages/sync-settings): 아톰 설정을 동기화 하는 패키지. 설정은 깃허브 gist에 업로드한다. settings에서 'Ignore EOL' 체크할 것.

## 기본 단축키

### 커서/포커스 이동

- <kbd>ctrl + 1</kbd>: 에디터로 포커스
- <kbd>ctrl + \ </kbd>: 파일 탐색창 토글
- <kbd>ctrl + shift + \ </kbd>: 파일 탐색창 열면서 현재 보고있는 파일의 위치로 이동
- <kbd>ctrl + k, 방향키</kbd>: 지정한 방향으로 pane 나누기
- <kbd>ctrl + k, ctrl + 방향키</kbd>: 지정한 방향의 pane으로 포커스
- <kbd>ctrl + r</kbd>: Toggle File Symbols
- <kbd>ctrl + shift + r</kbd>: Toggle Project Symbols

### 멀티 캐럿

Add Selection

- <kbd>ctrl + d</kbd>: 선택한 단어와 동일한 다음 단어에 캐럿 추가
- <kbd>ctrl + u</kbd>: 캐럿 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 캐럿
- <kbd>alt + f3</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 캐럿

### Git

- <kbd>ctrl + 8</kbd>: GitHub 창으로 포커스 이동
- <kbd>ctrl + shift + 8</kbd>: Git/GitHub창 토글
- <kbd>ctrl + 9</kbd>: Git 창으로 포커스 이동
- <kbd>ctrl + shift + 9</kbd>: Git/GitHub창 토글
- <kbd>alt + g, p</kbd>: push
