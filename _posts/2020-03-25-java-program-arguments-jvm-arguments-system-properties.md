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

## Program Arguments

'Java Command Line Arguments'가 더 정확한 이름이다. 자바 프로그램을 실행할 때 Main Class에 전달하는 인자를 의미한다.

아래처럼 쉘 명령어를 입력하면:

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

'J' 빼고 VM Arguments라고 부르기도 하며, `java` 명령어에 붙이는 모든 파라미터에 해당한다.

```bash
java MainTest -classpath C:/somewhere -Dspring.profiles.active=real -XX:+UseG1GC -Xms1024m -Xmx2048m -Duser.name=noritersand
```

여기서 `-`로 시작하는 이런 얘들이 다 JVM Arguments임.

## System Properties

System properties는 JVM arguments 중 `-D` 옵션으로 설정한 `이름=값` 형태의 설정값이다.

```bash
java MainTest -classpath C:/somewhere -Dspring.profiles.active=real -XX:+UseG1GC -Xms1024m -Xmx2048m -Duser.name=noritersand
```

이 중 `-Dspring.profiles.active=real`, `-Duser.name=noritersand` 얘네들이 System properties인데, 사실 이것 외에 `java.vm.name`, `java.home`, `os.version`, `user.home` 같은 JVM 내장 프로퍼티도 포함한다.

System properties는 `System` 객체의 `getProperty()`, `getProperties()` 메서드로 얻을 수 있다.

```java
public class TryEverything {
  public static void main(String[] args) {
    Properties props = System.getProperties();
    Enumeration<Object> enumerator = props.keys();
    while (enumerator.hasMoreElements()) {
      Object ele = enumerator.nextElement();
      String key = ele.toString();
      System.out.println(key + ": " + System.getProperty(key));
    }
  }
}
```
