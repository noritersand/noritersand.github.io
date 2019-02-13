---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Unix/Linux: 현재 시간 출력'
categories:
  - unix/linux
tags:
  - os
  - unix
  - linux
  - date
---

## 콘솔에 출력
```bash
echo `date +%Y-%m-%d` `date +%H:%M:%S`
```

## 현재 시간을 파일명으로
```bash
touch `date +%Y-%m-%dT%H:%M:%S`
```
