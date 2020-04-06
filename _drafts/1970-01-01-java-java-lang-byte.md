---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[Java] java.lang.Byte'
categories:
  - java
tags:
  - java
  - byte
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://docs.oracle.com/javase/9/docs/api/java/lang/Byte.html](http://docs.oracle.com/javase/9/docs/api/java/lang/Byte.html)

Byte 타입 자료형을 지원하는 클래스. byte 타입은 일반적으로 입출력, 전송 시에 사용한다. 변수에 Byte 혹은 byte 타입을 선언할 경우 할당할 수 있는 범위는 -128 ~ 127이다.
다음은 Byte 클래스의 메서드는 아니지만 바이트 배열과 문자열 사이의 형변환 예다:
```java
public static void main(String[] args) {
    String s = "대한민국";

    byte[] b = s.getBytes(); // 문자열을 바이트배열에 할당

    for (byte bb : b) {
        System.out.println(bb);
    }

    String ss = new String(b); // 바이트배열을 다시 문자열로..
    System.out.println(ss);
}
```
