---
layout: post
date: 2018-12-18 21:32:54 +0900
title: 'Visual Studio Code: 초기 설정'
categories:
  - devtool
tags:
  - devtool
  - visual studio code
  - vscode
---

atom 1.32.0 기준.

## 기본 설정

#### 자동완성 설정 변경

json으로 된 설정 파일을 열어서(`ctrl+shift+p` > 'Preferences: Open Settings (JSON)' > `엔터`) 아래 항목 추가:

```json
"editor.quickSuggestions": {
    "other": false,
    "comments": false,
    "strings": false
}
```

#### 타이틀에 파일 전체 경로 표시

settings(`ctrl+,`)에서 'window.title' 검색 후 입력란에 아래 추가:

```
${activeEditorLong}${separator}${rootName}
```

## 단축키

커맨드 팔레트`ctrl+shift+p`에서 'Preferences: Open Keyboard Shortcuts (JSON)' 입력 후 엔터. 이 후 열리는 keybindings.json을 아래처럼 변경:

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

## 기억해둘 기본 단축키

- Trigger Parameter Hints: `ctrl + shift + space`

## 플러그인

- change-case: wmaurer.change-case
- Java Server Pages (JSP): pthorsson.vscode-jsp
- open in browser: techer.open-in-browser
- Open file: fr43nk.seito-openfile
- Bookmarks:
- Auto Close Tag: formulahendry.auto-close-tag
- Go to Next/Previous Member: mishkinf.goto-next-previous-member
