---
layout: post
date: 2018-07-06 16:59:40 +0900
title: 'Git: 파일 무시하기 .gitignore'
categories:
  - git
tags:
  - git
  - gitignore
---

#### 참고 문서
- [https://git-scm.com/book/ko/v1/Git의-기초-수정하고-저장소에-저장하기#파일-무시하기](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%88%98%EC%A0%95%ED%95%98%EA%B3%A0-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0#%ED%8C%8C%EC%9D%BC-%EB%AC%B4%EC%8B%9C%ED%95%98%EA%B8%B0)

.gitignore 파일을 만들고 무시할 파일패턴을 명시하면 패턴에 따라 해당파일을 git이 자동으로 추가하거나 추적하지 않게 된다.
```bash
*.[oa]
*~
```
첫번쨰 줄은 확장자가 .o 나 .a인 파일을 Git이 무시하라는 것이고 둘째 줄은 ~로 끝나는 모든 파일을 무시하라는 것이다.

## Pattern
- 아무것도 없거나, `#`으로 시작하는 줄은 무시한다(코멘트 처리 가능).
- 표준 Glob패턴을 사용한다.
- 디렉토리는 슬래시`/`를 끝에 사용하는 것으로 표현한다.
- 느낌표`!`로 시작하는 패턴의 파일은 무시하지 않는다.
- `*`: 문자가 하나도 없거나 하나 이상
- `[abc]`: a, b, c 중 하나
- `?`: 문자 하나
- `[0~9]`: 캐릭터 사이에 있는 문자 하나

## example
```bash
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉토리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉토리 아래의 모든 .txt 파일을 무시
doc/**/*.txt
```
