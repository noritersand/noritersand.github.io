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

## 기본 설정

#### Suggestions 기능 설정 변경

Suggestions(IntelliSense)는 매우 좋은 기능이긴 하지만, 기본값 그대로 사용하기엔 약간 번거로운 면이 있다.

- Settings<kbd>ctrl + ,</kbd>에서 'Accept Suggestion On Commit Character'를 체크헤제하면 세미콜론`;`이나 소괄호`()` 등의 입력에 반응하지 않는다.
- Settings<kbd>ctrl + ,</kbd>에서 'Accept Suggestion On Enter'를 `off`로 변경하면 오직 <kbd>Tab</kbd>키에 의해서만 추천단어가 선택된다.
- Show All Commands<kbd>ctrl + shift + p</kbd>에서 'Preferences: Open Settings (JSON)' 입력 후 열리는 setting.json에 아래를 추가한다. 이렇게 하면 추천창이 자동으로 나타나지 않는다:

  ```json
  "editor.quickSuggestions": {
      "other": false,
      "comments": false,
      "strings": false
  }
  ```

개인적으론 세 번째 방법으로 꺼놓는게 편한듯.

#### 타이틀에 파일 전체 경로 표시

Settings<kbd>ctrl + ,</kbd>에서 'window.title' 검색 후 입력란에 아래 추가:

```
${activeEditorLong}${separator}${rootName}
```

#### 들여쓰기 설정 변경

Settings<kbd>ctrl + ,</kbd>에서 'indentation'검색 후 `Detect Indentation`은 체크 해제.  
이 후 `Insert Spaces`나 `Tab Size`는 취향껏...

#### 파일 제외하기

Settings<kbd>ctrl + ,</kbd>에서 'exclude' 검색 후 추가하면 된다. `Files: Exclude`는 Explorer에서 표시 제외, `Search: Exclude`는 빠른 열기와 검색에서 제외임.

## 팁

#### Code Snippet 추가하기

[공식 도움말 링크](https://code.visualstudio.com/docs/editor/userdefinedsnippets)  

Show All Commands<kbd>ctrl + shift + p</kbd>에서 `Preferences: Configure User Snippets` 입력 후 원하는 영역(전역인지, 현재 파일 전용인지, 특정 언어 전용인지...)을 선택하면 json 파일이 하나 열리는데, 여기에 다음처럼 작성하면:

```js
{
  "Print to console": {
    "prefix": "log",
    "body": [
      "console.log('$1');",
      "$2"
    ],
    "description": "Log output to console"
  }
}
```

`prefix` 'log'에 작동한다. 'Print to console'은 자동완성 창에 보여질 설명이다.

`body`의 내용은 여러 줄일 수 있으며, `$1`와 `$2`는 탭으로 이동가능한 위치를 의미한다. 위에는 없지만 `$0`이 있는데 이건 탭으로 이동할 최종 위치다. 탭 이동 순서는 `$1` > `$2` > `$0` 순인데, 이럴 거면 그냥 3으로 하지 왜 0인지는 아직 몲. `${1:text}` 이런식으로 작성할 수도 있는데, 일종의 placeholder 역할을 한다.

## 작성자 저장용 단축키 설정

Show All Commands에서 'Preferences: Open Keyboard Shortcuts (JSON)' 입력. 그리고 열리는 keybindings.json을 아래처럼 변경:

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
    "key": "ctrl+shift+k",
    "command": "editor.action.duplicateSelection"
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
  }
]
```

## 추천 확장 기능(플러그인)

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

### Debugger for Firefox(firefox-devtools.vscode-firefox-debug)

안써봤지만 파폭이니 일단 적어둠. 설명을 보면 파폭용 디버거라는데 실제로 어떻게 작동하는지는 몲겠다.

## 기본 단축키 메모

- <kbd>ctrl + k, ctrl + s</kbd>: 키보드 단축키 설정창 열기
- <kbd>ctrl + shift + space</kbd>: Trigger Parameter Hints
- <kbd>ctrl + k, ctrl + q<kbd>: Go to Last Edit Location
- <kbd>ctrl + m</kbd>: Toggle Tab Key Moves Focus. 탭 키의 들여쓰기/내어쓰기 기능을 비활성화하고 포커스 이동만 가능하도록 변환.

### 멀티 커서

- <kbd>ctrl + d</kbd>: 선택한 단어 기준 멀티 커서
- <kbd>ctrl + u</kbd>: 멀티 커서 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 커서
- <kbd>ctrl + shift + l</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 커서
- <kbd>alt + shift + i</kbd>: add cursors to line ends, 선택한 영역에서 각 라인마다 커서 분리한다.

### 그 외

- <kbd>ctrl + k, m</kbd>: Change Language Mode. Syntax 변경
- <kbd>ctrl + k, ctrl + s</kbd>: Open Keyboard Shortcuts. 단축키 목록 열기
- <kbd>ctrl + k, ctrl + i</kbd>: Show Hover. documentation popup 띄우기(함수의 JS Doc 같은거 보기)

---

- <kbd></kbd>:
