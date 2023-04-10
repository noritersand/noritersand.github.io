---
layout: post
date: 2022-08-08 22:01:43 +0900
title: '[java] 롬복 Project Lombok'
categories:
  - java
tags:
  - java
  - lombok
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://projectlombok.org/](https://projectlombok.org/)

#### 버전 정보

- x.x.x


## 개요

어쩌구

TODO


## @Builder

빌더 패턴으로 클래스를 초기화할 때 사용한다.

```java
@Data
@Builder
class BuildMe {
    private String a;
    private int b;
}

...

BuildMe instnc = BuildMe.builder().a("yo").b(123).build();
```

근데 이렇게 쓰다보면 마이바티스라던지 스프링 바인딩 등에서 기본 생성자가 없다며 에러가 발생할 수 있다. 따라서 기본생성자를 만들어주는 `@NoArgsConstructor`와 함께 쓴다.

그런데 또 이렇게만 하면 `@Builder` 어노테이션에서 에러를 뿜뿜하며 작동하지 않으므로 `@AllArgsConstructor` 어노테이션도 덧붙여야 한다. 🤣

그래서 결론은 빌더 패턴을 쓰려면 `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor` 세 개를 다 쓰는게 속 편하다는 것.

```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
class BuildMe {
    private String a;
    private int b;
}
```

### 상속 관계에서의 @Builder

슈퍼 클래스에만 `@Builder`가 있으면:

```java
    @Builder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Parent {
        private String a;
        private int b;
    }

    @Data
    public static class Child extends Parent {
        private Double c;
    }

    @Test
    public void testBuilder() {
        Child child = Child.builder().c(2.5).build(); // Cannot resolve method 'c' in 'ParentBuilder'

        Parent parent = Child.builder().build();
        Child child = (Child) parent; // java.lang.ClassCastException: class Parent cannot be cast to class Child
    }
```

`Child.builder()`는 `Parent.builder()`를 가리킨다. 따라서 만들어지는 인스턴스의 타입은 `Parent`이고 서브 클래스인 `Child`로의 캐스팅도 불가능하다.

반대로 서브 클래스에만 `@Builder`가 있으면:

```java
    @Builder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Child extends Parent {
        private Double c;
    }

    @Data
    public static class Parent {
        private String a;
        private int b;
    }

    @Test
    public void testBuilder() {
        Child child = Child.builder().a("ㅁㄴㅇ").build(); // Cannot resolve method 'a' in 'ChildBuilder'
    }
```

이번에는 `Child.builder()`가 호출되긴 하지만 슈퍼 클래스의 필드는 여전히 초기화할 수 없다.

그럼 양쪽에 다 있으면?

```java
    @Builder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Parent {
        private String a;
        private int b;
    }

    @Builder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Child extends Parent {
        private Double c;
    }

    @Test
    public void testSuperBuilder() {
        Child child = Child.builder().build();
        // java: builder() in Child cannot hide builder() in Parent. return type Child.ChildBuilder is not compatible with Parent.ParentBuilder
    }
```

이렇게 하면 `Parent`를 상속하는 `Child.builder()`가 `Parent.builder()`를 숨기려(hide 혹은 오버라이드)하는데 두 메서드의 반환 타입이 달라서 실패한다.

이렇듯 상속에서의 Builder는 많은 문제를 야기하는데, 이를 해결하기 위한 어노테이션이 따로 존재한다. 바로 아래를 보자.

### @SuperBuilder

상속 관계에서의 여러 문제를 해결하기 위한 롬복 어노테이션. 슈퍼/서브 양쪽에 모두 선언해야 함:

```java
    @SuperBuilder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Child extends Parent {
        private Double c;
    }

    @SuperBuilder
    @NoArgsConstructor
    @AllArgsConstructor
    @Data
    public static class Parent {
        private String a;
        private int b;
    }

    @Test
    public void testSuperBuilder() {
        Child child = Child.builder()
                .a("aaa")
                .b(123)
                .c(256.789).build();

        assertEquals(child.getB(), 123); // success
    }
```

어느 한 쪽이 `@Builder`면 작동하지 않으니 주의할 것.

