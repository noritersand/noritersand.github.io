---
layout: post
date: 2013-10-16 15:56:00 +0900
title: '[Java] instanceof'
categories:
  - java
tags:
  - java
  - operator
  - instanceof
---

* Kramdown table of contents
{:toc .toc}

instanceof는 자바에서 제공하는 연산자의 일종으로, 참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 사용한다.

```java
A instanceof b // A가 B클래스로 만들어진 인스턴지인지 여부를 boolean 값으로 반환
```

instanceof 연산자는 실제 인스턴스와 일치하는 타입의 연산 이외에 조상타입의 연산에도 true를 결과로 반환한다.

가령 다음과 같을 때 :

```java
class Parent {
    protected String txt = "하이";
}

class Child extends Parent {
    public void sayWhoyouare() {
        System.out.println("Class Child");
    }
}

class Child2 extends Parent {
    public void sayWhoyouare() {
        System.out.println("Class Child2");
    }    
}
```

Child 클래스는 Parent 클래스를 상속하며 Parent 클래스는 Object 클래스를 상속하니 모두 true로 출력된다.

```java
public class MainTest {
    public static void main(String[] args) {

        Child child = new Child();

        System.out.println(child instanceof Child); // true
        System.out.println(child instanceof Parent); // true
        System.out.println(child instanceof Object); // true
    }
}
```

여기서 instanceof 연산의 결과값이 true 일 경우는 곧 타입 변환이 가능하다는 것을 알 수 있다.

```java
public class MainTest {
    public static void main(String[] args) {

//        Parent parent = new Child();
        Parent parent = new Child2();
        m01(parent);

    }

    public static void m01(Parent parent) {

        if (parent instanceof Child) {
            Child child = (Child)parent;
            child.sayWhoyouare();

        } else if (parent instanceof Child2) {
            Child2 child2 = (Child2)parent;
            child2.sayWhoyouare();
        }
    }
}
```
