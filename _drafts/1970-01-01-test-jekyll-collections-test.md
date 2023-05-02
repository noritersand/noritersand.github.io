---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[test] jekyll collections test'
categories:
  - test
tags:
  - draft-permanantly
  - test
  - jekyll
  - blog
---

`_config.yml`에서 아래처럼:

```bash
collections:
  myawesomedirectory:
    output: true
    permalink: /:collection/:name
```

추가하면 `_myawesomedirectory`라는 폴더의 모든 글을 `/myawesomedirectory/파일명` 경로로 퍼블리싱함.
벗뜨 이런식으로 작성한 글은 index 페이지에 표시가 안되서 일단 보류
