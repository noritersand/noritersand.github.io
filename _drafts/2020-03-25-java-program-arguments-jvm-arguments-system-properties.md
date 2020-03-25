---
layout: post
date: 2020-03-25 17:08:00 +0900
title: '[Java] Program Arguments, JVM Arguments, System Properties'
categories:
  - java
tags:
  - java
  - program-arguments
  - jvm-arguments
  - system-properties
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](somewhere)

JVM arguments와 System properties는 동의어가 아니다. JVM arguments는 `java` 명령어 뒤에 붙이는 모든 옵션들을 말하며, System properties는 JVM arguments 중 `-D` 옵션으로 추가한 `이름=값` 형태의 설정값을 말한다. System properties는 `java.class.version`, `java.home`, `java.vendor` 같은 JVM의 기본 설정값도 포함한다.

program arguments: main 클래스의 전달 인자
```bash
java MainTest a 1 b c 3 4 d f
```

JVM arguments(=시스템 프로퍼티<sup>system properties</sup>): java 실행 할 때 `java` 명령어의 `-D` 옵션으로 지정하는 값
```bash
java MyApp -Dspring.profiles.active=real
```

JVM arguments는 `java` 명령어 뒤에 붙이는 모든 옵션들을 말하며, System properties는 JVM arguments 중 `-D` 옵션으로 설정한 `이름=값` 형태의 설정값이다.

## Program Arguments

'Java Command Line Arguments'라고도 함. 프로그램 아규먼트는 자바 프로그램을 실행할 때 Main Class에 전달하는 인자를 의미한다.

이렇게 전달하면:

```bash
java MainTest a 1 b c 3 4 d f
```

이렇게 받는다:

```java
public class MainTest {
  public static void main(String[] args) {
    for (String arg : args) {
      System.out.println(arg); // a 1 b c 3 4 d f 출력
    }
  }
}
```

## JVM Arguments

## System Properties


## JVM Arguments와 System Properties의 차이
