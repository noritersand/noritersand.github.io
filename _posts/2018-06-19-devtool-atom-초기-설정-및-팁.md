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

atom 1.45.0 기준.

## 기본 설정

#### 맞춤법 검사 비활성화

`Settings` > `Installed Packages` > `spell-check`에서 `disable` 클릭

#### 자동완성 설정 변경

`Settings` > `Installed Packages` > `autocomplete-plus`의 설정에서 `Show Suggestions On Keystroke`를 체크해제한다. 이 항목이 체크 되어 있으면 타이핑 할 때마다 자동완성 목록이 나타나는데 이게 꽤 귀찮다. 그냥 필요할 때 단축키로 사용할 것.  
자동완성 단축키: <kbd>ctrl + space</kbd>

#### 마크다운 미리보기 설정 변경

`Settings` > `Installed Packages` > `markdown-preview`의 설정에서:

- `Live Update` 체크 해제: 글이 길 때 미리보기가 자동갱신되면 렉이 심함.
- `Open Preview In Split Pane` 체크 해제: 새 탭으로 열리는게 더 편함.

#### 붙여넣기 시 자동 들여쓰기 해제

`Settings` > `Editor`에서 `Auto Indent On Paste` 체크 해제.

#### 개행, 탭, 공백 문자 표시

`Settings` > `Editor`에서 `Show Invisibles` 체크.

#### 탭 타입 변경

`Settings` > `Editor`에서 `Tab Type`을 `soft`로 변경. 이 옵션은 탭 문자를 스페이스바로 변경함을 의미한다.

## 작성자 저장용 단축키 설정

커맨드 팔레트<kbd>ctrl + shift + p</kbd>에서 'Application: Open Your Keymap' 입력 후 엔터. 그리고 열리는 keymap.cson을 아래처럼 변경:

```yml
'.platform-win32, .platform-win32 .command-palette atom-text-editor':
  'ctrl-shift-P': 'command-palette:toggle'
  'f1': 'command-palette:toggle'

'atom-workspace':
  'ctrl-i': 'nothing'
  'ctrl-alt-i': 'nothing'
  'alt--': 'goto-last-edit:back'
  'alt-shift--': 'goto-last-edit:forward'

'atom-workspace atom-text-editor:not([mini])':
  'ctrl-shift-k': 'editor:duplicate-lines'

'atom-text-editor:not([mini])':
  'ctrl-shift-d': 'editor:delete-line'
  'ctrl-shift-up': 'editor:move-line-up'
  'ctrl-shift-down': 'editor:move-line-down'
  'f5': 'nothing'
  'f9': 'sort-lines:case-insensitive-sort'
  'ctrl-f9': 'sort-lines:sort'

'atom-text-editor':
  # 'ctrl-up': 'keyboard-scroll:scrollUp'
  # 'ctrl-down': 'keyboard-scroll:scrollDown'
  'ctrl-up': 'nothing'
  'ctrl-down': 'nothing'
  'alt-up': 'nothing'
  'alt-down': 'nothing'
  'alt-shift-i': 'editor:split-selections-into-lines'
```

## 추천 패키지(플러그인)

- ~~[keyboard-scroll](https://atom.io/packages/keyboard-scroll)~~: ~~다른 에디터처럼 키보드로 스크롤만 한 줄씩 이동할 때 필요함.~~  
  멀티 커서 증식(?)키<kbd>ctrl + alt + up/down</kbd>가 이상작동하는 현상이 있음.
- [url-encode](https://atom.io/packages/url-encode): ?
- [goto-last-edit](https://atom.io/packages/goto-last-edit): <kbd>ctrl + i</kbd> 혹은 <kbd>ctrl + alt + i</kbd>로 마지막 수정 이력 이동... 인데 서브라임이랑 같도록 바꿔서 씀. 키 바꿔서 쓸꺼면 해당 패키지 settings에서 Keybindings는 끌 것
- [sort-lines](https://atom.io/packages/sort-lines): 아톰에 없는 sorting 기능 추가. 기본 단축키는 <kbd>f5</kbd>지만 서브라임이랑 키 같게 수정함.
- [sync-settings](https://atom.io/packages/sync-settings): 아톰 설정을 동기화 하는 패키지. 설정은 깃허브 gist에 업로드한다.

## 기본 단축키 메모

### 커서/포커스 이동

- <kbd>ctrl + 1</kbd>: 에디터로 포커스
- <kbd>ctrl + \\</kbd>: 파일 탐색창 토글
- <kbd>ctrl + shift + \\</kbd>: 파일 탐색창 열면서 현재 보고있는 파일의 위치로 이동
- <kbd>ctrl + k, 방향키</kbd>: 지정한 방향으로 pane 나누기
- <kbd>ctrl + k, ctrl + 방향키</kbd>: 지정한 방향의 pane으로 포커스

### 멀티 커서

- <kbd>ctrl + d</kbd>: 선택한 단어 기준 멀티 커서
- <kbd>ctrl + u</kbd>: 멀티 커서 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 커서
- <kbd>alt + f3</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 커서

### Git

- <kbd>ctrl + 8</kbd>: GitHub 창으로 포커스 이동
- <kbd>ctrl + shift + 8</kbd>: Git/GitHub창 토글
- <kbd>ctrl + 9</kbd>: Git 창으로 포커스 이동
- <kbd>ctrl + shift + 9</kbd>: Git/GitHub창 토글
- <kbd>alt + g, p</kbd>: push

---

- <kbd></kbd>:
