---
layout: post
date: 2018-12-18 21:32:54 +0900
title: '[devtool] Visual Studio Code(vscode) 초기 설정 및 팁'
categories:
  - devtool
tags:
  - devtool
  - visual-studio-code
  - vscode
---

* Kramdown table of contents
{:toc .toc}


## 기본 설정

### Suggestions 기능 설정 변경

Suggestions(IntelliSense)는 매우 좋은 기능이긴 하지만, 기본값 그대로 사용하기엔 약간 번거로운 면이 있다.

- Settings<kbd>ctrl + ,</kbd>에서 'Accept Suggestion On Commit Character'를 체크해제하면 세미콜론`;`이나 소괄호`()` 등의 입력에 반응하지 않는다.
- Settings<kbd>ctrl + ,</kbd>에서 'Accept Suggestion On Enter'를 `off`로 변경하면 오직 <kbd>Tab</kbd>키에 의해서만 추천단어가 선택된다.
- 자동으로 나타나는 추천창이 귀찮으면 Show All Commands<kbd>ctrl + shift + p</kbd>에서 'Preferences: Open Settings (JSON)' 입력 후 열리는 setting.json에 아래를 추가한다:

  ```json
  "editor.quickSuggestions": {
    "other": false,
    "comments": false,
    "strings": false
  }
  ```

### 타이틀에 파일 전체 경로 표시

Settings<kbd>ctrl + ,</kbd>에서 'window.title' 검색 후 입력란에 아래 추가:

```
${activeEditorLong}${separator}${rootName}
```

### 들여쓰기 설정 변경

Settings<kbd>ctrl + ,</kbd>에서 'indentation'검색 후:

- `Detect Indentation`은 체크 해제
- `Insert Spaces`나 `Tab Size`는 취향껏...
- `Editor: Auto Indent`는... 뭘로 바꾸긴 해야 하는데 일단 `Full`은 아님

### 파일 제외하기

Settings<kbd>ctrl + ,</kbd>에서 'exclude' 검색 후 추가하면 된다. `Files: Exclude`는 Explorer에서 표시 제외, `Search: Exclude`는 빠른 열기와 검색에서 제외임.


## 팁

### 코드 스니펫 추가하기

[공식 도움말 링크](https://code.visualstudio.com/docs/editor/userdefinedsnippets)  

Show All Commands<kbd>ctrl + shift + p</kbd>에서 `Preferences: Configure User Snippets` 입력 후 원하는 영역(전역인지, 현재 파일 전용인지, 특정 언어 전용인지...)을 선택하면 json 파일이 하나 열리는데, 여기에 다음처럼 작성하면:

```js
{
  "console.log": {
    "prefix": "cl",
    "body": [
      "console.log('${1:msg}:', $2);"
    ],
    "description": "Log output to console"
  }
}
```

`prefix` 'cl'에 자동 완성이 반응한다. 'console.log'는 자동 완성 창에 보여질 설명이다.

`body`의 내용은 여러 줄일 수 있으며, `$1`와 `$2`는 탭으로 이동가능한 위치를 의미한다. 위에는 없지만 `$0`이 있는데 이것은 탭으로 이동할 최종 위치다. 탭 이동 순서는 `$1 > $2 > $0` 순인데, 이럴 거면 그냥 3으로 하지 왜 0인지는 몲. `${1:msg}`에서 `msg`는 문자 그대로 출력되는 일종의 placeholder다.

### 작성자 저장용 JavaScript User Snippet

```json
{
  "console.log": {
    "prefix": "cl",
    "body": [
      "console.log(${1});"
    ],
    "description": "Log output to console"
  },
  "console.log 2": {
    "prefix": "cl2",
    "body": [
      "console.log('${1:selectme}:', ${2:selectme});"
    ],
    "description": "Log output to console"
  },
  "console.debug": {
    "prefix": "cd",
    "body": [
      "console.debug(${1});"
    ],
    "description": "Log output to console"
  },
  "console.debug 2": {
    "prefix": "cd2",
    "body": [
      "console.debug('${1:selectme}:', ${2:selectme});"
    ],
    "description": "Log output to console"
  },
  "JS Doc: author and since": {
    "prefix": "@auto",
    "body": [
      "@author fixalot",
      "* @since $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE$1"
    ]
  },
  "current time": {
    "prefix": "!now",
    "body": [
      "$CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND$1"
    ]
  }
}
```


## 추천 확장 기능(플러그인)

- ESLint `dbaeumer.vscode-eslint`: JS 분석 도구. 문법 오류 검출 같은 기능이 있음.
- Prettier - Code formatter `esbenp.prettier-vscode`: code formatter임. JS, JSON, CSS, HTML, Markdown 등을 지원
- change-case `wmaurer.change-case`
- Java Server Pages (JSP) `pthorsson.vscode-jsp`
- open in browser `techer.open-in-browser`
- Open file `fr43nk.seito-openfile`
- Bookmarks `alefragnani.bookmarks`
- Auto Close Tag `formulahendry.auto-close-tag`
- Go to Next/Previous Member `mishkinf.goto-next-previous-member`: 전과 후의 멤버(함수, 메서드, 프로퍼티, 지역변수 등)로 이동하는 기능을 추가한다. 윈도우일 경우 기본 단축키는 <kbd>ctrl + 방향키 위/아래</kbd>임.
- Highlight Matching Tag `vincaslt.highlight-matching-tag`
- Bracket Pair Colorizer 2 `coenraads.bracket-pair-colorizer-2`
- indent-rainbow `oderwat.indent-rainbow`
- Remote - WSL `ms-vscode-remote.remote-wsl`: WSL을 사용한다면 필요한 플러그인. 요거 설치하면 WSL 내의 프로젝트를 VSCODE로 열 수 있음.
- Naming-Shiba `fredkeemhaus.Naming-Shiba`: 한국어 문장을 영문변수명으로 바꿔주는 플러그인. 노매드코더 슬랙 참가자 중 한 명이 말듦. 단축키는 <kbd>ctrl + shift + a</kbd>와 <kbd>ctrl + shift + s</kbd>인데 다른 거랑 겹치니까 딴 걸로 바꿔야 함.
- GitHub Copilot `github.copilot`: AI가 코드를 작성해주는 쩌는 플러그인. 단축키는 발동 <kbd>alt + \\</kbd>, 제안 선택 <kbd>tab</kbd>, 자동 완성 제안 창 보기 <kbd>ctrl + enter</kbd>, 코파일럿 빠른 채팅 열기 <kbd>ctrl + shift + i</kbd>, GitHub Copilot View 포커싱 <kbd>alt + shift + p</kbd>가 있다(마지막은 없어서 추가한 거).
- Auto Import `steoates.autoimport`: import 구문을 자동으로 작성해주는 개꿀 익스텐션.


## 작성자 저장용 단축키 설정

Show All Commands에서 'Preferences: Open Keyboard Shortcuts (JSON)' 입력하면 keybindings.json이 열리는데, 내용을 아래처럼 변경한다:

```json
[
  {
    "key": "ctrl+shift+d",
    "command": "-workbench.view.debug"
  },
  {
    "key": "ctrl+shift+d",
    "command": "editor.action.deleteLines",
    "when": "textInputFocus && !editorReadonly"
  },
  {
    "key": "ctrl+shift+k",
    "command": "-editor.action.deleteLines",
    "when": "textInputFocus && !editorReadonly"
  },
  {
    "key": "ctrl+k s",
    "command": "-workbench.action.files.saveAll"
  },
  {
    "key": "ctrl+shift+s",
    "command": "-workbench.action.files.saveAs"
  },
  {
    "key": "ctrl+shift+s",
    "command": "workbench.action.files.saveAll"
  },
  {
    "key": "ctrl+j",
    "command": "-workbench.action.togglePanel"
  },
  {
    "key": "ctrl+shift+j",
    "command": "editor.action.joinLines"
  },
  {
    "key": "ctrl+shift+down",
    "command": "-cursorDownSelect",
    "when": "textInputFocus"
  },
  {
    "key": "ctrl+shift+up",
    "command": "-cursorUpSelect",
    "when": "textInputFocus"
  },
  {
    "key": "ctrl+shift+down",
    "command": "gotoNextPreviousMember.nextMember"
  },
  {
    "key": "ctrl+down",
    "command": "-gotoNextPreviousMember.nextMember"
  },
  {
    "key": "ctrl+shift+up",
    "command": "gotoNextPreviousMember.previousMember"
  },
  {
    "key": "ctrl+up",
    "command": "-gotoNextPreviousMember.previousMember"
  },
  {
    "key": "ctrl+alt+o",
    "command": "workbench.action.openWorkspace"
  },
  {
    "key": "alt+oem_3",
    "command": "workbench.action.compareEditor.focusOtherSide"
  },
  {
    "key": "shift+alt+p",
    "command": "workbench.panel.chat.view.copilot.focus"
  },
  {
    "key": "ctrl+shift+oem_2",
    "command": "editor.action.blockComment",
    "when": "editorTextFocus && !editorReadonly"
  },
  {
    "key": "shift+alt+a",
    "command": "-editor.action.blockComment",
    "when": "editorTextFocus && !editorReadonly"
  }
]
```

`oem_3`는 [사용자의 키보드 레이아웃에 따라 다를 수 있는데](https://github.com/microsoft/vscode/issues/27491) 작성자의 경우 백틱``` ` ```에 해당함.


## 기본 단축키

### 파일 에디터

- <kbd>ctrl + .</kbd>: Quick Fix...
- <kbd>ctrl + k, ctrl + q<kbd>: Go to Last Edit Location
- <kbd>ctrl + k, ctrl + i</kbd>: Show Hover. documentation popup 띄우기(함수의 JS Doc 같은거 보기)
- <kbd>f12</kbd>: Go To Definition. 선언부로 이동
- <kbd>shift + f12</kbd>: Go To References. 함수 등을 참조하고 있는 코드로 이동(혹은 작은 팝업으로 보여줌)
- <kbd>ctrl + shift + r</kbd>: Refactor... 현재 캐럿이 위치에 따라 가능한 코드 리팩터링 옵션을 보여준다.
- <kbd>ctrl + shift + right/left</kbd>: 단어(words) 단위의 선택 영역을 좌우로 확장한다. 단어란 언더바`_`를 포함한 연속적인 단어 구성 문자를 의미한다. (abc, abcDef, ABC_DEF, ...)
- <kbd>alt + shift + right/left</kbd>: Expand/Shrink Selection. 단어보다 더 넓은 범위로 선택 영역을 확장하거나 좁힌다. (서브라임 텍스트에서는 보조 단어(subwords) 단위의 선택 영역을 좌우로 확장하는 단축키임)

### 멀티 캐럿

Add Selection

- <kbd>ctrl + d</kbd>: 선택한 단어와 동일한 다음 단어에 캐럿 추가
- <kbd>ctrl + u</kbd>: 캐럿 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 캐럿
- <kbd>ctrl + shift + l</kbd>: Select All Occurrences of Find Match. 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 캐럿
- <kbd>alt + shift + i</kbd>: add cursors to line ends 선택한 영역에서 각 라인마다 캐럿 분리

### Code Folding

- <kbd>ctrl + shift + [</kbd>: Fold. 코드 접기
- <kbd>ctrl + shift + ]</kbd>: Unfold. 펼치기
- <kbd>ctrl + k, ctrl + 0</kbd>: Fold All. 모두 접기
- <kbd>ctrl + k, ctrl + j</kbd>: Unfold All. 모두 펼치기

### 그 외

- <kbd>ctrl + k, m</kbd>: Change Language Mode. Syntax 변경
- <kbd>ctrl + k, ctrl + s</kbd>: Open Keyboard Shortcuts 단축키 목록 열기
- <kbd>shift + alt + .</kbd>: Auto Fix... 에러가 발생했을 때 어떻게 수정할 지 선택지를 제시해 준다.
- <kbd>ctrl + shift + space</kbd>: Trigger Parameter Hints
- <kbd>ctrl + m</kbd>: Toggle Tab Key Moves Focus 탭 키의 들여쓰기/내어쓰기 기능을 비활성화하고 포커스 이동만 가능하도록 변환.
