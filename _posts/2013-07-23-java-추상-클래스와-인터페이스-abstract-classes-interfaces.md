---
layout: post
date: 2013-07-23 21:00:00 +0900
title: '[Java] 추상 클래스와 인터페이스, abstract classes, interfaces'
categories:
  - java
tags:
  - java
  - abstract
  - interface
  - implements
---

* Kramdown table of contents
{:toc .toc}

## abstract

```
abstract class 클래스명 {
    abstract 접근제어자 리턴타입 메서드명();  
}
```

`abstract` 키워드를 사용하여 추상 클래스나 추상 메서드를 선언할 수 있다. 추상 클래스란 추상 메서드를 선언할 수 있는 클래스를 말하며 추상 메서드는 선언부만 존재하고 구현부가 없는 메서드를 말한다.

클래스에 추상 메서드가 없어도 `abstract`를 사용할 수 있으나 추상 클래스는 인스턴스를 생성할 수 없다는 점을 기억하자. 이는 인터페이스와 동일하다.

```java
abstract class Player {
    boolean pause;
    abstract void play();
    abstract void stop();
    abstract void pause();
}

class CDPlayer extends Player {
    @Override
    void play() {
        ...
    }

    @Override
    void stop() {
        ...
    }

    @Override
    void pause() {
        ...
    }
}
```

추상 클래스를 상속받은 클래스를 구현할 때 추상 메서드가 존재한다면 반드시 해당 메서드를 오버라이드 해야 한다.

```java
public class Test {
    public static void main(String[] args) {

        Parent parent = new Parent(); // compile error: Cannot instantiate the type Parent
        Parent.a = "1123";
        Parent.m01(); // 클래스 메서드

        Child child = new Child();
        child.b = "abcd";
        child.m02(); // 인스턴스 메서드
    }
}

abstract class Parent {
    public static String a;
    public String b;

    public static void m01() {
        System.out.println("클래스 메서드");
    }

    public void m02() {
        System.out.println("인스턴스 메서드");
    }
}

class Child extends Parent {
    // have nothing
}
```

추상 클래스는 클래스 멤버와 인스턴스 멤버를 모두 소유할 수 있다. 이 중 클래스 멤버는 일반 클래스와 동일하게 사용하지만 인스턴스 멤버의 경우 추상 클래스의 인스턴스를 생성할 수 없는 문제가 있으므로 항상 추상 클래스를 상속받은 자식 클래스를 통해 접근해야 한다.

## interface

```
interface 인터페이스명 {
    public static final 변수타입 변수명 = 값;
    public 메서드명();
}
```

인터페이스는 추상 클래스와 매우 흡사하지만 추상 클래스와 달리 인스턴스 변수를 선언할 수 없으며 오직 추상 메서드와 final 클래스 변수만 선언할 수 있다. 인터페이스에서 추상 메서드는 `abstract` 키워드를 생략해도 된다.

## implements

인터페이스를 구체화한 클래스에서 사용하는 키워드. 구체화 클래스는 인터페이스의 추상 메서드를 반드시 재정의해야한다. `extends`와 `implements` 모두 상속의 개념이다. 단, 클래스는 클래스끼리 `extends` 해야하며 인터페이스는 인터페이스끼리 `extends` 해야한다.

```
class Parent { }
class Child extends Parent { }

interface I_Parent { }
interface I_Child extends I_Parent { }

interface I_Parent { }
class Child implements I_Parent { }
```
