---
layout: post
date: 2013-07-23 21:00:00 +0900
title: 'Java: 상속 extends'
categories:
  - java
tags:
  - java
  - extends
---

* Kramdown table of contents
{:toc .toc}

자바에서 상속이란 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것을 말한다. 상속은 코드의 재사용성을 높이고 코드 중복을 제거하려는 목적으로 사용된다.

```java
class 클래스명 extends 부모 클래스명 {
    ...
}
```

`extends` 키워드를 클래스 선언부에 명시하여 상속을 구현한다. `extends` 뒤에 오는 클래스가 부모 클래스다. 자식 클래스는 부모 클래스의 생성자와 클래스 초기화 블록을 제외한 모든 멤버, 즉 변수와 메서드를 상속받는다.

```java
public class Test2 {
    public static void main(String[] args) {
        Parent parent = new Parent("hello");
        System.out.println(parent.getA()); // hello
        System.out.println(parent.getB()); // i'm not hello

        Child child = new Child("hello"); // compile error: The constructor Child(String) is undefined

        Child child2 = new Child();
        System.out.println(child2.getA()); // null
        System.out.println(child2.getB()); // i'm not hello
    }
}

class Parent {
    private String a;
    private String b;

    public Parent() {}

    {
        this.b = "i'm not hello";
    }

    static {
        // 클래스 초기화 블록은 상속되지 않는다.
        System.out.println("initializing");
    }

    public Parent(String a) {
        // 생성자는 상속되지 않는다.
        this.a = a;
    }

    public String getA() {
        return this.a;
    }

    public String getB() {
        return this.b;
    }
}

class Child extends Parent {
    // have nothing
}
```

```java
class Parent {
    String str = "부모멤버";
    public void show() {
        System.out.println("부모의 메서드");
    }
}

class Child extends Parent {
    String str = "자식멤버";

    @Override
    public void show() {
        System.out.println(super.str);
        System.out.println(this.str);

        super.show(); // 부모의 메서드, 즉 재정의 하기 전의 메서드를 의미함.
        // this.show();  // 무한재귀호출
    }
}

-> new Child().show()
-> 부모멤버
-> 자식멤버
-> 부모의 메서드
```

자식 클래스의 인스턴스를 생성할 땐 자손의 멤버와 부모의 멤버가 모두 합쳐진 하나의 인스턴스가 생성된다. **이때 원칙상 자식 클래스의 생성자에 부모 클래스의 생성자를 호출하는 코드인 `super()`가 호출되어야 하지만** `super()`가 없으면 컴파일러가 자동으로 추가해주므로 신경 쓰지 않아도 된다.

부모 클래스에 선언된 인스턴스 변수와 같은 이름의 인스턴스 변수를 자손 클래스에 중복 정의했을 때, `super`가 참조하는 멤버와 `this`가 참조하는 멤버가 다르다.

다음을 보면 :

```java
public class TestEverything {
    public static void main(String[] args) {
        Child child = new Child();
        child.show();
    }
}

class Parent {
    String str = "부모에서 정의된 인스턴스 변수";
    String txt = "부모 txt";
}

class Child extends Parent {
    String txt = "자식 txt";

    public void show() {
        str = "자식이 재정의 한 인스턴스 변수";
        System.out.println(super.str);
        System.out.println(this.str);  // (1) 중복 정의되지 않은 경우

        System.out.println(super.txt);
        System.out.println(this.txt);  // (2) 중복 정의된 경우

        String str = "지역변수";
        System.out.println(str);  // (3) 인스턴스 변수와 같은 이름의 지역변수를 생성할 경우
    }
}

-> 자식이 재정의 한 인스턴스 변수
-> 자식이 재정의 한 인스턴스 변수
-> 부모 txt
-> 자식 txt
-> 지역변수
```

부모 쪽에서만 정의된 인스턴스 변수 `str`은 `super`, `this` 모두 자식의 인스턴스 변수를 참조하지만 부모와 자식 양 쪽에서 중복 정의된 인스턴스 변수 `txt`는 `super`, `this` 각각 자신의 인스턴스 변수를 참조한다. `str`처럼 클래스 타입을 명시한 뒤 같은 이름으로 변수를 초기화하면 그 변수는 더 이상 인스턴스 변수가 아닌 지역변수를 참조한다. 따라서 위의 경우처럼 같은 이름의 지역변수가 선언될 경우엔 `this` 없이는 인스턴스 변수에 접근할 수 없다.

부모 클래스에 추상 클래스가 존재한다면 해당 메서드는 반드시 재정의 해야한다. 그렇지 않을경우 컴파일 에러가 발생할 것이다.

```java
abstract class Parent {
    abstract void print();
    public void show() { }
}

class Child extends Parent {
    @Override
    void print() { }
}
```

스태틱 메서드는 재정의 할 수 없다. 하지만 접근은 가능:

```java
class Parent {
    public static void stM() {
        System.out.println("ㅎㅇ");
    }
}

class Child extends Parent {
    public void show() {
        stM();
    }
}
```

부모타입의 참조변수에 자식의 인스턴스를 할당할 시 재정의`override` 된 멤버에 한해서 자식의 멤버가 실행우선권을 갖는다:

```java
public class TestEverything {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show(); // 자식 메서드
    }
}

class Parent {
    public String show() {
        return "부모 메서드";
    }
}

class Child extends Parent {
    @Override
    public String show() {
        return "자식 메서드";
    }
}
```
