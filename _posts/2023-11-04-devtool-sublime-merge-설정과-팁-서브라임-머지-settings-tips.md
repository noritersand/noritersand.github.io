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

서브라임 머지 기본 설정값과 팁을 작성하는 글.


## 검색 기능 활용하기

```bash
# some-config.ts 파일의 히스토리 중 변경 내용에 foobar가 포함된 커밋만 표시
file:"some-config.ts" and contents:"foobar"

# 78095e 커밋에서 README.md 파일의 100-150 라인 변경점 표시
commit:78095e and file:README.md line:100-150

# 73333a 커밋 기준, ddl.sql 파일 719-801 라인의 변경점 보기
file:"ddl.sql" line:719-801 from:73333a
```

**TODO** `from:`의 정확한 용도를 모름.

- 검색 값(위 예시에서 "foobar"에 해당)은 따옴표를 생략하거나 큰따옴표를 사용해야 한다.
- 필드 연산자를 생략하고 값만 입력하면 커밋 메시지를 검색한다.
- `and`는 생략할 수 있다. 
- `bug fix`는 "bug"와 "fix"가 모두 포함된 커밋 메시지를 검색한다.
- `"bug fix"`는 
- `line:`은 `file:`의 보조 연산자라서 `and`를 붙이면 작동하지 않는다.

#### 연산자 목록

논리 연산자:

- `and`
- `or`
- `not`

필드 연산자: 

- `author:`
- `branch:`
- `path:`
- `file:`
- `line:`
- `from:`
- `min-parents:`
- `max-parents:`
- `commit:`
- `tree:`
- `contents:`
- `before:`
- `after:`

기타 연산자: 

- `is-visible`
- `()`


## 기타

현재(🗓️ 2022-05-04) 공식 문서에서 command 목록을 찾을 수가 없다. 그래서 [누군가 답답해서 만들어버린 걸](https://github.com/Sublime-Instincts/CommandsBrowser) 패키지로 설치해서 확인해야 함.


끗.
