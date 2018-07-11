---
title: 'Unix: 현재 시간 출력'
categories:
  - os
  - unix/linux
tags:
  - todo
  - unix
  - linux
---

## 콘솔에 출력
```bash
echo `date +%Y-%m-%d` `date +%H:%M:%S`
```

## 현재 시간을 파일명으로
```bash
touch `date +%Y-%m-%dT%H:%M:%S`
```
