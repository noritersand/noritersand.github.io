---
layout: post
date: 2017-04-03 17:57:00 +0900
title: '[Java] 제어자, modifier'
categories:
  - java
tags:
  - java
  - modifier
  - access
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html
- https://www.w3schools.com/java/java_modifiers.asp


## 접근 제어자 access modifier

| Modifier    | Class | Package | Subclass |  World |
|-------------|-------|---------|----------|--------|
| private     | O     | X       | X        | X      |
| no modifier | O     | O       | X        | X      |
| protected   | O     | O       | O        | X      |
| public      | O     | O       | O        | O      |

- private: 같은 클래스 내에서만 접근 가능
- no modifier(default, package-private): 같은 패키지 내에서만 접근 가능
- protected: 같은 패키지이거나 상속받은 클래스에서만 접근 가능
- public: 접근 제한 없음


## 제어자 작성 순서

[http://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.3.1](http://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.3.1)

#### For fields:

```
Annotation public protected private static final transient volatile
```

#### For methods:

```
Annotation public protected private abstract static final synchronized native strictfp
```
