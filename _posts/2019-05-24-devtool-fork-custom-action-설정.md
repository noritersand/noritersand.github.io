---
layout: post
date: 2019-05-24 14:50:00 +0900
title: '[devtool] Fork custom action 설정'
categories:
  - devtool
tags:
  - devtool
  - fork
  - git-gui
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://fork.dev/blog/](https://fork.dev/blog/)

## assume unchanged

- target: `file action`
- script path: `$git`
- parameters: `update-index --assume-unchanged $filepath`

변경 목록 화면에서 우클릭 - 커스텀 액션 실행. 단, 파일 하나씩만 가능.
