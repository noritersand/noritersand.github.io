---
layout: post
date: 2019-03-06 18:24:00 +0900
title: '[Java] static import'
categories:
  - java
tags:
  - java
  - import
  - static import
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](/somewhere)

```
import static org.junit.Assert.*;
```

#### 이클립스를 사용한다면...

import all을 작성하고 이클립스의 Organize Imports(ctrl+shift+o) 기능을 실행하면 `*`가 없어지고 스태틱 변수 하나하나씩 모두 import 된다. 이 현상을 방지하려면:

`Windows` > `Preferences` > `Java` > `Code Style` > `Organize Imports`에서 `Number of static imports needed for .*`를 1로 변경한다.
