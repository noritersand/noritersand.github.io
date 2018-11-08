---
layout: post
date: 2013-07-26 23:04:00 +0900
title: 'Java: java.lang.Class<T>'
categories:
  - java
tags:
  - java
  - class
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서
- [http://docs.oracle.com/javase/10/docs/api/java/lang/Class.html](http://docs.oracle.com/javase/10/docs/api/java/lang/Class.html)
- [http://docs.oracle.com/javase/10/docs/api/java/lang/reflect/Method.html](http://docs.oracle.com/javase/10/docs/api/java/lang/reflect/Method.html)
- [http://docs.oracle.com/javase/10/docs/api/java/lang/reflect/Field.html](http://docs.oracle.com/javase/10/docs/api/java/lang/reflect/Field.html)


## java.lang.Class

Class 클래스는 특정 클래스나 인터페이스에 관한 메타정보를 검색할 수 있는 메서드를 포함하고 있다. T는 이 Class 객체에 의해 모델화 되는 클래스의 형태로 예를 들어 String 클래스의 형태는 `Class<String>`이다. 모델화 되는 클래스가 명확하지 않는 경우 `Class<?>`라고 명시한다.

대부분의 메서드가 `sun.java.Reflection`을 통해 작동한다.

## 주요 메서드

### forName()

```
static Class<?> forName( String className ): Returns the Class object associated with the class or interface with the given string name.

static Class<?> forName( String name, boolean initialize, ClassLoader loader ): Returns the Class object associated with the class or interface with the given string name, using the given class loader.

T newInstance(): Creates a new instance of the class represented by this Class object.
```

`forName("[클래스]").newInstance()`는 new 연산자가 수행하는 인스턴스 생성과 결과가 같다. 다만 `newInstance()`는 결과 타입이 제네릭이기 때문에 형변환을 명시해야 한다:

```java
public class LogicTest {
    public static void main(String[] args) throws Exception {

        Temp t = new Temp();
        t.showText();

        Temp t2 = (Temp) Class.forName("com.test.Temp").newInstance();
        t2.showText();
    }
}

class Temp {
    public void showText() {
        System.out.println("하이");
    }
}

-> 하이
-> 하이
```

## example

### JDBC 커넥션 드라이버 생성

```java
public static Connection getConnection() {
    String url="jdbc:oracle:thin:@220.76.176.66:1521:orcl", user="noritersand", pwd="java301$!";
    if(conn == null) {
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            conn = DriverManager.getConnection(url, user, pwd);
        } catch (Exception e) {
            System.out.println(e);
        }
    }
    return conn;
}
```

### 클래스의 메타정보 구하기

```java
Class cls = Class.forName("java.lang.String");

// 클래스의 메서드 리스트 출력
Method[] mthdArry = cls.getMethods();
for (Method ele : mthdArry) {
    System.out.println(ele); // 해당 클래스의 메서드를 출력할 수 있다.
}

// 특정 메서드 정보 가져오기 #1
Method mthd = cls.getMethod("toString", new Class[0]);
System.out.println(mthd.getName());
System.out.println(mthd.getModifiers());
System.out.println(mthd.getReturnType());

// 특정 메서드 정보 가져오기 #2
Method mthd2 = cls.getMethod("substring", new Class[]{int.class, int.class});
System.out.println(mthd2.getName());
System.out.println(mthd2.getModifiers());
System.out.println(mthd2.getReturnType());
```

### 클래스와 객체생성, 메서드 호출

```java
import java.lang.reflect.Method;

public class LogicTest {
    public static void main(String[] args) throws Exception {

        // Demo7 d = new Demo7()
        Class cls = Class.forName("com.test.Demo7");

        Object ob = cls.newInstance(); // 객체생성

        Demo7 d = (Demo7) ob;
        d.setName("자바");
        d.write();

        // 메서드 정의
        Method m1 = cls.getDeclaredMethod("add", new Class[] { int.class, int.class });
        Method m2 = cls.getDeclaredMethod("plus", new Class[] { Integer.class, Integer.class });
        Method m3 = cls.getDeclaredMethod("print", new Class[] { String.class, int.class });
        Method m4 = cls.getDeclaredMethod("write", null);

        // 메서드 호출
        Object o1 = m1.invoke(ob, new Object[] { 10, 20 });
        System.out.println(o1);

        // 메서드 호출2
        m3.invoke(ob, new Object[] { "합 : ", o1 });
    }
}

class Demo7 {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public void write() {
        System.out.println(name);
    }

    public int add(int a, int b) {
        return a + b;
    }

    public Integer plus(Integer a, Integer b) {
        return a + b;
    }

    public void print(String title, int a) {
        System.out.println(title + a);
    }
}
```
