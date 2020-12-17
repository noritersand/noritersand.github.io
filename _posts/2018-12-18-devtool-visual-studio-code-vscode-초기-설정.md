---
layout: post
date: 2018-12-18 21:32:54 +0900
title: '[devtool] Visual Studio Code(vscode): 초기 설정'
categories:
  - devtool
tags:
  - devtool
  - visual-studio-code
  - vscode
---

## 기본 설정

#### 자동완성 설정 변경

json으로 된 설정 파일을 열어서(<kbd>ctrl + shift + p</kbd> > 'Preferences: Open Settings (JSON)' 입력 후 엔터) 아래 항목 추가:

```json
"editor.quickSuggestions": {
    "other": false,
    "comments": false,
    "strings": false
}
```

#### 타이틀에 파일 전체 경로 표시

settings<kbd>ctrl + ,</kbd>에서 'window.title' 검색 후 입력란에 아래 추가:

```
${activeEditorLong}${separator}${rootName}
```

#### 파일 제외하기

settings<kbd>ctrl + ,</kbd>에서 'exclude' 검색 후 추가하면 된다. `Files: Exclude`는 Explorer에서 표시 제외, `Search: Exclude`는 빠른 열기와 검색에서 제외임.

## 작성자 저장용 단축키 설정

커맨드 팔레트<kbd>ctrl + shift + p</kbd>에서 'Preferences: Open Keyboard Shortcuts (JSON)' 입력 후 엔터. 그리고 열리는 keybindings.json을 아래처럼 변경:

```json
[
    {
        "key": "ctrl+shift+d",
        "command": "-workbench.view.debug"
    },
    {
        "key": "ctrl+shift+k",
        "command": "-editor.action.deleteLines",
        "when": "textInputFocus && !editorReadonly"
    },
    {
        "key": "ctrl+shift+d",
        "command": "editor.action.deleteLines",
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
- Go to Next/Previous Member `mishkinf.goto-next-previous-member`
- Highlight Matching Tag `vincaslt.highlight-matching-tag`
- Bracket Pair Colorizer 2 `coenraads.bracket-pair-colorizer-2`
- indent-rainbow `oderwat.indent-rainbow`

### Debugger for Firefox(firefox-devtools.vscode-firefox-debug)

안써봤지만 파폭이니 일단 적어둠. 설명을 보면 파폭용 디버거라는데 실제로 어떻게 작동하는지는 몲겠다.

## 기본 단축키 메모

- <kbd>ctrl + shift + space</kbd>: Trigger Parameter Hints
- <kbd>ctrl + k, ctrl + q<kbd>: Go to Last Edit Location

### 멀티 커서

- <kbd>ctrl + d</kbd>: 선택한 단어 기준 멀티 커서
- <kbd>ctrl + u</kbd>: 멀티 커서 추가 되돌리기
- <kbd>ctrl + alt + 방향키 위/아래</kbd>: 위나 아래로 멀티 커서
- <kbd>ctrl + shift + l</kbd>: 현재 파일에서 선택한 단어와 같은 모든 단어에 멀티 커서
- <kbd>alt + shift + i</kbd>: add cursors to line ends, 선택한 영역에서 각 라인마다 커서 분리한다.

---

- <kbd></kbd>:
