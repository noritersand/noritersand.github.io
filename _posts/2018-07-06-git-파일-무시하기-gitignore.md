---
layout: post
date: 2018-07-06 16:59:40 +0900
title: '[Git] 파일 무시하기 .gitignore'
categories:
  - git
tags:
  - git
  - gitignore
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Git - 수정하고 저장소에 저장하기](https://git-scm.com/book/ko/Git의-기초-수정하고-저장소에-저장하기#파일-무시하기)

.gitignore 파일을 만들고 무시할 파일패턴을 명시하면 패턴에 따라 해당파일을 git이 자동으로 추가하거나 추적하지 않게 된다.

```bash
*.[oa]
*~
```

첫 번째 줄은 확장자가 .o 나 .a인 파일을 Git이 무시하라는 것이고 둘째 줄은 ~로 끝나는 모든 파일을 무시하라는 것이다.


## Pattern

⚠️ Git 자체 Glob 패턴으로 Ant 패턴의 다중 선택 구분자(`,`)는 지원하지 않는다.

- 코멘트 라인은 `#`으로 시작한다.
- 표준 Glob패턴을 사용한다.
- 디렉터리는 슬래시`/`를 끝에 사용하는 것으로 표현한다.
- 느낌표`!`로 시작하는 패턴의 파일은 무시하지 않는다.
- `*`: 문자가 하나도 없거나 하나 이상
- `[abc]`: a, b, c 중 하나
- `?`: 문자 하나
- `[0~9]`: 캐릭터 사이에 있는 문자 하나


## example

```bash
# 확장자가 .a인 파일 무시
*.a

# qwer 디렉터리나 파일을 모든 현재 경로와 하위 경로에서 무시. **/qwer 이라 생각해도 됨
qwer

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉터리에 있는 AAA파일은 무시하고 subdir/AAA처럼 하위디렉터리에 있는 파일은 무시하지 않음
/AAA

# build/ 디렉터리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉터리 아래의 모든 .txt 파일을 무시
doc/**/*.txt
```


## 주의 사항

이미 버전 관리 대상으로 등록한(= 추적 중인) 파일은 .gitignore에 추가해도 아무런 변화가 없다. 이럴 때는 `rm --cached` 명령으로 실제 파일은 그대로 둔 채 Git의 관리 대상에서만 지우는 방법을 쓴다:

```bash
git rm --cached FILE_NAME
```
