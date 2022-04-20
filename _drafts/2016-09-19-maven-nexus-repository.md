---
layout: post
date: 2016-09-19 12:20:28 +0900
title: '[Maven] nexus repository'
categories:
  - maven
tags:
  - buildtool
  - maven
  - nexus
  - maven-repository
---

Manager 접속:

```bash
cd nexus-repo\nexus-2.12.0-01\bin
nexus install
nexus start
```

localhost:7070/nexus

admin / admin123


#### simplecaptcha 배포:

`Repositories > 3rd party > Artifact Upload`

```xml
<dependency>
  <groupId>nl.captcha</groupId>
  <artifactId>simplecaptcha</artifactId>
  <version>1.2.1</version>
  <type>pom</type>
</dependency>
```

jar 업로드
