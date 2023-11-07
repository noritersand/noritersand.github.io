---
layout: post
date: 2023-11-04 18:05:35 +0900
title: '[devtool] Sublime Merge 설정과 팁'
categories:
  - devtool
tags:
  - devtool
  - sublimemerge
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.sublimemerge.com/docs/](https://www.sublimemerge.com/docs/)

#### 버전 정보

- Build 2xxx


## 개요

서브라임 머지 기본 설정값과 팁, 단축키에 대한 기록.

**TODO**


## 기본 설정

**TODO**


## 검색 기능 활용하기

```bash
# 73333a498cc22070f090993b5287b90cb5b5cb22 커밋 기준, edupass-ddl.sql 파일 719-801 라인의 변경점 보기
file:"database/ddl-2/edupass-ddl.sql" line:719-801 from:73333a498cc22070f090993b5287b90cb5b5cb22
```

**TODO** `line` 옵션에 대한 연구 필요


## 작성자 저장용 사용자 설정

#### key bindings - user

현재(2022-05-04) 공식 문서에서 command 목록을 찾을 수가 없다. 그래서 [누군가 답답해서 만들어버린 걸](https://github.com/Sublime-Instincts/CommandsBrowser) 패키지로 설치해서 확인해야 함.

```json
[
  { "keys": ["f1"], "command": "show_command_palette" },
  { "keys": ["ctrl+shift+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },
  { "keys": ["ctrl+shift+k"], "command": "duplicate_line" },
  { "keys": ["ctrl+p"], "command": "quick_switch_repository" },
  { "keys": ["ctrl+alt+shift+a"], "command": "stage_all" },
  { "keys": ["ctrl+alt+shift+u"], "command": "unstage_all" },
  { "keys": ["ctrl+alt+shift+d"], "command": "discard_all_modified" },
  { "keys": ["ctrl+,"], "command": "open_preferences" }
  // { 
  //   "keys": ["ctrl+alt+shift+enter"], 
  //   "command": "commit", 
  //   "args": { "mode": "commit --amend" }, 
  //   "context": [
  //     { "key": "setting.commit_message" }, 
  //     { "key": "can_commit" }
  //   ]
  // }
]
```

- `stage_all`은 untracked 파일도 같이 스테이징하는 명령이다. 
- `discard_all_modified`은 모든 변경 사항을 취소하니 주의해서 사용할 것. 
- <kbd>ctrl + alt + shift + enter</kbd>는 리베이스인 amend commit인데, 실수하면 위험한 기능이라 막아놨음.
