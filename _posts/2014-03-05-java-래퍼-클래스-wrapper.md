---
layout: post
date: 2014-03-05 15:47:00 +0900
title: 'Java: 래퍼 클래스 wrapper'
categories:
  - java
tags:
  - java
  - wrapper
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://javarevisited.blogspot.com/2015/09/difference-between-primitive-and-reference-variable-java.html](https://javarevisited.blogspot.com/2015/09/difference-between-primitive-and-reference-variable-java.html)

기본형(primitive, 원시형) 타입의 변수를 객체로 다루기 위해 만들어진 클래스를 래퍼(wrapper) 클래스라고 한다. 가령 int 타입의 래퍼 클래스는 Integer고 실제 코드는 다음처럼 구성되어 있다:

```java
public final class Integer extends Number implements Comparable {
    private int value;

    // 생략
}
```

| primitive type | wrapper class | 생성자 | example |
|----------------|---------------|---------------------------------------------------------|------------------------------------------------------------------------------------|
| boolean | Boolean |  Boolean(boolean value) Boolean(String s) |  `Boolean b = new Boolean(true);`<br>`Boolean b = new Boolean("true");` |
| char | Character |  Character(char value) |  `Character c = new Character('a');`   |
| byte | Byte |  Byte(byte value) Byte(String s) |  `Byte b = new Byte((byte) 8);`<br>`Byte b = new Byte("8");` |
| short | Short |  Short(short value) Short(String s) |  `Short s = new Short((short) 10);`<br>`Short s = new Short("10");`  |
| int | Integer |  Integer(int value) Integer(String s) |  `Integer i = new Integer(10);`<br>`Integer i = new Integer("10");` |
| long | Long |  Long(long value) Long(String s) |  `Long l = new Long(10L);`<br>`Long l = new Long("10");` |
| float | Float |  Float(double value) Float(float value) Float(String s) |  `Float f = new Float(1.0);`<br>`Float f = new Float(1.0F);`<br>`Float f = new Float("1.0F");` |
| double | Double |  Double(double value) Double(String s) |  `Double d = new Double(1.0);`<br>`Double d = new Double("1.0");` |

래퍼 클래스는 형변환 메서드로 `parse()`와 `valueOf()`를 제공한다. `parse()`는 전달받은 문자열을 기본형으로 변환해 돌려주지만 `valueOf()`는 기본형이 아닌 래퍼 클래스로 변환한다는 차이점이 있다.

#### 문자열 -> 기본형

```java
int num = Integer.parseInt("65536");
```

#### 문자열 -> 래퍼

```java
Integer num = Integer.valueOf("65536");
```

JDK 1.5 이상에서 자바는 기본형과 래퍼 클래스간의 오토박싱(autoboxing), 오토언박싱(autounboxing)을 제공한다.

다음을 보면:

```java
int num = 10;
Integer number = num;
```

int 타입 변수인 num을 형변환 없이 Integer 타입변수에 할당해도 컴파일 에러가 발생하지 않는다. 원칙적으로는 틀린 코드지만 자바 컴파일러가 다음처럼 코드를 자동으로 변경하며, 이를 오토박싱이라 한다:

```java
int num = 10;
Integer number = Integer.valueOf(num);
```

오토언박싱도 마찬가지인데 다음처럼 래퍼 타입 변수에 기본형 값을 할당해도 컴파일 에러가 발생하지 않는다:

```java
Integer number = 10;
int num = number;
```

이 역시 컴파일러가 다음처럼 자동변환하기 때문이다:

```java
Integer number = 10;
int num = number.intValue();
```
