---
layout: post
date: 2026-07-21 10:30:30 +0900
title: '[misc] 앤트 경로 패턴 Ant Path Style Patterns'
categories:
  - misc
tags:
  - misc
  - ant-pattern
---

* Kramdown table of contents
{:toc .toc}


## 개요

2000년대 초반 Apache Ant 프로젝트에서 파일 묶음을 관리하기 위해 고안된 규칙으로, 특정 패턴에 맞는 파일을 한 번에 선택하기 위해 만들어졌다.


## 상세

- `?`: 단일 문자 와일드카드. 정확히 한 개의 문자와 일치하는 파일을 찾는다. 해당 자리에 문자가 없으면 제외한다. (`t?st`는 `tst`와 일치하지 않음)
- `*`: 단일 깊이 와일드카드. 한 디렉터리 깊이 내에서 모든 문자와 일치하는 파일을 찾는다. 파일명 패턴일 때 문자가 없어도 포함하지만(`t*st`는 `tst`와 일치함), 디렉터리 패턴일 때는 해당 위치에 반드시 하나의 디렉터리가 있어야 한다(`/*/*.java`는 `/Abc.java`와 일치하지 않음).
- `**`: 무제한 깊이 와일드카드. `*`에서 디렉터리 제한이 없어진 와일드카드로 현재와 모든 하위 디렉터리에서 탐색한다. 재귀적 와일드카드라고도 한다. `/**.java`와 `/**/*.java`는 비슷해보여도 환경에 따라 다른 결과가 나올 수 있으니 `/**/*.java` 패턴으로 작성하는 게 좋다.
- `,`: 다중 선택 구분자. 둘 이상의 패턴을 OR 조건으로 적용한다. 패턴 내에서 선택지로 사용할 때는 중괄호(`{}`) 내에 작성해야 하지만, 여러 패턴을 나열할 때는 중괄호를 생략한다.

```bash
# com/test/ 디렉터리에 있는 모든 .java 파일을 선택. 하위 디렉터리는 탐색하지 않음
com/test/*.java

# com/test/ 디렉터리 바로 아래의 디렉터리에 있는 .java 파일을 모두 선택
com/test/*/*.java

# com/test/ 디렉터리와 모든 깊이의 하위 디렉터리에 있는 .java 파일을 모두 선택
com/test/**/*.java

# org/apache/ 아래 어디든 상관없이 파일명이 Test로 시작하고 뒤에 딱 한 글자만 더 붙는 .java 파일 선택 (e.g.: org/apache/util/Test1.java, org/apache/core/ui/TestA.java)
org/apache/**/Test?.java

# 프로젝트 루트부터 모든 하위 디렉터리를 통틀어 이름이 pom.xml인 파일은 모두 포함
**/pom.xml

# src/main 아래의 js나 ts 디렉터리에 있는 모든 파일 선택
src/main/{js,ts}/*

# js 혹은 ts 파일 모두 선택
 *.js,*.ts
```

ℹ️ Windows Terminal 같은 일부 환경에서는 `,`를 패턴의 일부로 인식되지 않아서 패턴 전체를 따옴표로 감싸야 한다. (`npx repomix --include "**.xml,**.properties"`)
