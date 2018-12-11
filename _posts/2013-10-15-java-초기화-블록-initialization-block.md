---
layout: post
date: 2013-10-15 17:58:00 +0900
title: 'Java: 초기화 블록 Initialization Block'
categories:
  - java
tags:
  - java
  - initialization block
---

* Kramdown table of contents
{:toc .toc}

초기화 블록은 자주 사용되지는 않으나 복잡한 초기화에 사용되며 클래스 초기화 블록, 인스턴스 초기화 블록으로 구분된다.

```java
class 클래스명 {
    static {
        클래스 초기화 블록
    }
    {
        인스턴스 초기화 블록
    }
}
```

클래스 초기화 블록은 클래스가 메모리에 로딩될 때, 인스턴스 초기화 블록은 생성자가 호출되었을 때 실행된다. 그리고 생성자보다 인스턴스 초기화 블록이 우선 실행된다.

```java
class Init {
    static {
        System.out.println("ㅎㅇ");
    }

    {
        System.out.println("선발대");
    }

    public Init() {
        System.out.println("후발대");
    }
}

public class MainTest {
    public static void main(String[] args) {
        Init init = new Init();
    }
}

// ㅎㅇ
// 선발대
// 후발대
```

다음 코드에서 클래스 초기화 블록은 인스턴스를 여러번 생성해도 단 한번만 실행되며, 생성자와 관계없이 메모리 로딩 시점에 우선 실행됨을 확인 할 수 있다:

```java
class Init {
    static int a = 0;
    static {
        System.out.println("static { }");
    }
    {
        System.out.println("{ }");
    }
    public Init() {
        System.out.println("생성자");
    }
}

public class MainTest {
    public static void main(String[] args) {

        Init.a = 1;

        System.out.println("Init init = new Init();");
        Init init = new Init();

        System.out.println("Init init2 = new Init();");
        Init init2 = new Init();
    }
}

// static { }
// Init init = new Init();
// { }
// 생성자
// Init init2 = new Init();
// { }
// 생성자
```
