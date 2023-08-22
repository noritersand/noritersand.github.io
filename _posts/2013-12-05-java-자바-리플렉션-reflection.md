---
layout: post
date: 2013-12-05 17:11:00 +0900
title: '[Java] 자바 리플렉션 reflection'
categories:
  - java
tags:
  - java
  - resultsetmetadata
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Field.html](http://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Field.html)
- [http://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Method.html](http://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Method.html)
- [http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html](http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html)
- [https://docs.oracle.com/javase/tutorial/reflect/](https://docs.oracle.com/javase/tutorial/reflect/)
- [http://gyrfalcon.tistory.com/entry/Java-Reflection](http://gyrfalcon.tistory.com/entry/Java-Reflection)

## 개요

리플렉션이란 인스턴스로 클래스의 정보를 분석하기 위한 기법이면서, 자바에서 해당 기법을 구현한 클래스와 패키지의 통칭이다.

## 변수의 이름을 파라미터로 다루기

```java
public class Test {
    String person1 = "사람1";
    String person2 = "사람2";

    public static void main(String[] args) throws Exception {
        Test test = new Test();
    }
    void getValue(String who) throws Exception {
        System.out.println("show something..");
    }
}
```

String 타입의 인스턴스 변수인 person1, person2가 선언되어 있는 클래스가 있다. 그리고 그 필드의 값을 파라미터에 따라 선택하여 출력하는 `getValue()` 메서드를 구현한다고 하자.

일반적으로는 다음처럼 가장 간단하고 고전적인 방식인, 비교에 의한 분기문으로 구현할 수 있을것이다:

```java
void getValue(String who) throws Exception {
    if (who.equals("person1")) {
        System.out.println(this.person1);
    } else if (who.equals("person2")) {
        System.out.println(this.person2);
    }
}
```

만약 분기문을 사용하지 않고 Field 클래스를 이용해 변수명을 문자열로 취급하려면 다음처럼 작성한다:

```java
import java.lang.reflect.Field;

public class Test {
    String person1 = "사람1";
    String person2 = "사람2";

    public static void main(String[] args) throws Exception {
        Test test = new Test();
        test.getValue("person2");
    }

    void getValue(String who) throws Exception {
        Class<?> cls = this.getClass();
        Field person = cls.getDeclaredField(who);
        System.out.println(person.getName()); // person2
        System.out.println(person.get(this)); // 사람2
    }
}
```

```java
Test test = new Test();
Field target = test.getDeclaredField("내가 찾아야 할 녀섴");
```

인스턴스의 클래스값을 getClass로 가져와 getDeclaredField 메서드를 이용하면 해당 필드에 관한 정보를 가져올 수 있다.

```java
target.get(test);
```

이 정보는 Field 클래스 타입이며 여기서 getName 혹은 get을 이용하면 프로퍼티의 이름과 값을 구할 수 있다. 여기서 get메서드의 인자값으로는 같은 타입의 인스턴스 객체를 넘겨줘야한다.

## setAccessible()

```java
public void setAccessible(boolean flag)
```

`setAccessible()`은 필드나 메서드의 접근제어 지시자에 의한 제어를 변경한다.

일반적으로 private 인스턴스 변수나 메서드는 해당 클래스의 외부에서는 접근할 수 없다. 가령 다음처럼 private으로 지정된 `some` 변수에 접근하려고 하면 예외가 발생할 것이다.

```java
class AccessTest {
    private String some = "yo";
    private String getSome() {
        return this.some;
    }
}

public class MainClass {
    public static void main(String[] args) throws Exception {
      AccessTest accessTest = new AccessTest();

      System.out.println(accessTest.some); // compile error: The field AccessTest.some is not visible

      Field field = accessTest.getClass().getDeclaredField("some");
      System.out.println("field: " + field.get(accessTest));
      /*
       * runtime error:
       *     java.lang.IllegalAccessException:
       *     Class MainClass can not access a member of class AccessTest with modifiers "private"
       */
    }
}
```

이때 `setAccessible(true)`를 사용하면 문제 없이 접근할 수 있게 된다:

```java
class AccessTest {
    private String some = "yo";
    private String getSome() {
        return this.some;
    }
}

public class MainClass {
    public static void main(String[] args) throws Exception {
      AccessTest accessTest = new AccessTest();

      Field field = accessTest.getClass().getDeclaredField("some");
      field.setAccessible(true);
      System.out.println("field: " + field.get(accessTest));

//    System.out.println(accessTest.some); // 이것은 여전히 불가능하다.

      Method method = accessTest.getClass().getDeclaredMethod("getSome");
      method.setAccessible(true);
      System.out.println("method: " + method.invoke(accessTest, (Object[]) null));

//    System.out.println(accessTest.getSome()); // compile error: The method getSome() from the type AccessTest is not visible

      method.setAccessible(false);
      System.out.println("method: " + method.invoke(accessTest, (Object[]) null));
      /*
       * setAccessible(false)는 접근제어를 원래 상태로 돌려놓기 때문에 다음과 같은 런타임 에러가 발생할 것이다.
       * runtime error:
       *     java.lang.IllegalAccessException:
       *     Class PrivateVariableAcc can not access a member of class AccessTest with modifiers "private"
       */
    }
}
```

단, 이 방법은 reflect를 통한 필드나 메서드 접근에 한하며 접근연산자(.)를 통한 방식은 불가능하다.

## example

```java
import java.lang.reflect.Field;
import java.lang.reflect.Method;

import org.junit.Assert;
import org.junit.Test;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ReflectionTest {
    @SuppressWarnings("unused")
    private static final Logger log = LoggerFactory.getLogger(ReflectionTest.class);

    @Test
    public void test() throws Exception {
        Object instance = new MyClass();
        Class<?> clazz = instance.getClass();

        // invoke method
        Method method = clazz.getDeclaredMethod("myMethod");
        Assert.assertNotNull(method);
        Assert.assertEquals("finally you found me!", method.invoke(instance));

        // access field
        Field field = clazz.getDeclaredField("myField");
        Assert.assertNotNull(field);
        Assert.assertEquals(0, field.get(instance));
    }
}

class MyClass {
    public int myField = 0;

    public String myMethod() {
        return "finally you found me!";
    }
}
```
