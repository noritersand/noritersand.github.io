---
title: 'sublime text: 초기 설정'
date: 2018-04-17 13:32:10
categories:
  - tools
tags:
  - tools
  - sublime text
---

## package control 설치
ctrl+shift+p 로 팔레트를 열고 install package control

## 한글 인코딩 지원 패키지 설치
ctrl+shift+p 누른후 보이는 커맨드 창에서 install package [enter] > ConvertToUTF8

## 작성자 저장용 사용자 설정

#### settings - user
```json
{
	"auto_complete_commit_on_tab": true,
	"fallback_encoding": "UTF-8",
	"font_face": "Consolas",
	"font_size": 11,
	"show_encoding": true,
	"show_line_endings": true
}
```

#### key bindings - user
```json
[
	{ "keys": ["ctrl+shift+c"], "command": "toggle_comment", "args": { "block": false } },
	{ "keys": ["ctrl+shift+/"], "command": "toggle_comment", "args": { "block": true } },
	{ "keys": ["ctrl+k", "ctrl+k"], "command": "do_nothing" },
	{ "keys": ["ctrl+k", "ctrl+backspace"], "command": "do_nothing" },
	{ "keys": ["f9"], "command": "save_all" },
	{ "keys": ["f8"], "command": "sort_lines", "args": {"case_sensitive": false} },
	{ "keys": ["ctrl+f8"], "command": "sort_lines", "args": {"case_sensitive": true} }
]
```
