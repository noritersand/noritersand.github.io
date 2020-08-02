---
layout: post
date: 2013-10-21 15:05:00 +0900
title: '[Eclipse] JUnit with eclipse'
categories:
  - eclipse
tags:
  - devtool
  - eclipse
  - junit
  - java
  - code-test
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [http://lyb1495.tistory.com/73](http://lyb1495.tistory.com/73)
- [http://junit.sourceforge.net/javadoc/](http://junit.sourceforge.net/javadoc/)

```java
package com.test;

public class Number {
    private int value;

    public Number() {
        this(0);
    }

    public Number(int value) {
        this.value = value;
    }

    public int add(int rhs) {
        return value += rhs;
    }

    public int minus(int rhs) {
        return value -= rhs;
    }

    public int multiply(int rhs) {
        return value *= rhs;
    }

    public int divide(int rhs) {
        return value /= rhs;
    }

    public int getValue() {
        return value;
    }
}
```

위 클래스를 JUnit으로 테스트한다고 하고, 우선 라이브러리를 추가한다:

![](/images/junit-with-eclipse-1.png)

그리고 JUnit 테스트 케이스를 작성한다:

![](/images/junit-with-eclipse-2.png)

![](/images/junit-with-eclipse-3.png)

![](/images/junit-with-eclipse-4.png)

![](/images/junit-with-eclipse-5.png)

이후 자동으로 생성된 코드를 아래처럼 수정 후:

```java
package com.test;

import static org.junit.Assert.*;

import org.junit.Test;

public class NumberTest {
    @Test
    public void testNumber() {
        Number num = new Number();
        assertEquals(0, num.getValue());
    }

    @Test
    public void testNumberInt() {
        Number num = new Number(10);
        assertEquals(10, num.getValue());
    }

    @Test
    public void testAdd() {
        Number num = new Number(10);
        assertEquals(20, num.add(10));
    }

    @Test
    public void testMinus() {
        Number num = new Number(10);
        assertEquals(5, num.minus(5));
    }

    @Test
    public void testMultiply() {
        Number num = new Number(10);
        assertEquals(20, num.multiply(2));
    }

    @Test
    public void testDivide() {
        Number num = new Number(10);
        assertEquals(2, num.divide(5));
    }

    @Test
    public void testGetValue() {
        Number num = new Number(10);
        assertEquals(10, num.getValue());
    }
}
```

실행한다. 단축키는 <kbd>alt + shift + x</kbd>, <kbd>t</kbd>

![](/images/junit-with-eclipse-6.png)

고의로 틀린 테스트를 작성한 코드의 실행결과:

![](/images/junit-with-eclipse-7.png)

모든 코드가 성공하면 녹색막대기가 출력된다:

![](/images/junit-with-eclipse-8.png)
