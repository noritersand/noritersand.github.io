---
layout: post
date: 2020-12-26 08:39:12 +0900
title: '[Java] 메서드 참조 Method References'
categories:
  - java
tags:
  - java
  - method-references
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html](https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html)

#### 버전 정보

- Java 8 이상에서 사용 가능

Java 8부터는 익명 클래스를 정의할 때 람다 식을 이용하여 코드를 단축할 수 있는데, 특정 조건에 한해 메서드 이름만으로도 같은 기능을 하는 아주 획기적인 표현식도 같이 생겼다.

> ... 그런데, 가끔 람다 식은 기존 메서드(existing method)를 호출하는 것 외에는 아무 것도 하지 않는 경우도 있습니다. 이러한 경우 이름으로 기존 메서드를 참조하는 것이 더 명확해집니다. 메서드 참조를 사용하면 이 작업을 수행할 수 있습니다. 이미 이름이 있는 메서드에 대해 작고 읽기 쉬운 람다 식입니다.

콜백 구조의 예시를 들어보자. 가령 아래처럼 `Bar` 타입의 인스턴스를 받아 `say()`를 실행하는 `Foo`가 있다고 했을 때:

```java
interface Bar {
    void say(String str);
}

static class Foo {
    static void call(String txt, Bar callback) {
        callback.say(txt);
    }
}
```

Java 8 전에는 내부 클래스나 익명 클래스로 구현체(=인스턴스)를 만들었다면:

```java
final String txt = "Hello";
// 아래 넷은 모두 결과가 같다.

// #1 내부 클래스를 이용하는 방식
Bar callback = new Bar() {
    @Override
    public void say(String str) {
        logger.debug(str);
    }
};
Foo.call(txt, callback);

// #2 익명 클래스를 이용하는 방식
Foo.call(txt, new Bar() {
    @Override
    public void say(String str) {
        logger.debug(str);
    }
});
```

Java 8 이후에는 아래처럼 람다 표현식으로 대체할 수 있는데:

```java
// #3 1, 2를 줄여서 쓸 수 있게 나온게 람다 표현식
// Foo.call(txt, s -> logger.debug(s)); // 아래와 같음
Foo.call(txt, (s) -> {
    logger.debug(s);
});
```

여기서 한 술 더 뜬게 바로 **메서드 참조**다:

```java
// #4 3에서 더 줄이면 메서드 참조
Foo.call(txt, logger::debug);
Foo.call(txt, System.out::println);
```

단, 아무 람다를 메서드 참조로 바꿀 수 있는건 아니고, 구현체의 실행부에서 파라미터를 순서와 개수 그대로(말처럼 '그대로'다. 캐스팅이나 연산 따위도 안됨) 사용해야 하며, **한 줄의 표현식만 대체할 수 있다**.

메서드 참조를 사용할 수 없는 경우의 예시:

```java
InnerClass.call((str) -> {
    logger.debug(str);
    logger.info(str);
});
// 위의 경우 메서드 참조로 작성할 수 없음.
// 메서드 참조는, 람다 표현식의 바디에서 단 하나의 호출 표현식만 작성할 경우에만 적용할 수 있다.
InnerClass.call(logger::info;logger::debug); // Syntax error on token ";", , expected
```
