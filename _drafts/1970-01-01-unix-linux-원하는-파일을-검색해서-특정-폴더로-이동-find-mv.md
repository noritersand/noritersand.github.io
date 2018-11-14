---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Unix/Linux: 원하는 파일을 검색해서 특정 폴더로 이동(find + mv)'
categories:
  - os
  - linux
tags:
  - linux
  - find
  - mv
---

```
aaa_dir/a.png
aaa_dir/b.png
aaa_dir/bbb_dir/c.png
```
위를

```
zzz_dir/a.png
zzz_dir/b.png
zzz_dir/c.png
```
이렇게 옮기려고 할 때

find 명령어를 통해 옮기고자하는 파일들을 검색합니다. (다양한 옵션으로 검색가능):
```bash
find aaa_dir(절대경로 또는 상대경로) -name "*.png"
```

최종 파일명만 추려냅니다:
```bash
find aaa_dir -name "*.png" -printf "%f\n"
```

하고자하는 명령어 입력:
```bash
find aaa_dir -name "*.png" -printf "%f\n" -exec mv {} zzz_dir/ \;
```
