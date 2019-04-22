---
layout: post
date: 2013-07-23 12:59:00 +0900
title: 'Java: java.lang.Object'
categories:
  - java
tags:
  - java
  - object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서
- [http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html](http://docs.oracle.com/javase/7/docs/api/java/lang/Object.html)
- [http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html](http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)

Object 클래스는 모든 클래스의 상속계층도 상 가장 위에 위치하는 조상클래스다. 자바 컴파일러는 아무것도 상속하지 않는 클래스를 컴파일 할 때 자동으로 Object 클래스를 상속받도록 한다.

Object 클래스는 소유한 변수(=멤버변수)가 없고 11개의 메서드만 존재한다.

## 주요 메서드

### clone()

객체 자신의 복사본을 리턴한다. 이 메서드는 변수의 값을 단순 복사하기 때문에 참조값을 소유한 객체를 복사할 때는 주의해서 사용해야 한다. 또한 이 메서드를 사용할 경우 반드시 Cloneable 인터페이스를 implements 해야한다. 그렇지 않으면 CloneNotSupportedException이 발생한다.

```java
public class LogicTest {
    public static void main(String... args) throws CloneNotSupportedException {
        Doppelganger c = new Doppelganger();
        System.out.println(c.getAa()); // 1234

        c.setAa("asdasd");

        Doppelganger cc = (Doppelganger) c.clone();
        System.out.println(cc.getAa()); // asdasd
    }
}

class Doppelganger implements Cloneable {
    private String aa = "1234";

    public String getAa() {
        return aa;
    }

    public void setAa(String aa) {
        this.aa = aa;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

### equals( Object obj )

객체 자신과 obj가 같은 객체인지 불리언값으로 리턴. 참조값을 비교하므로 동등비교 연산자`==`와 결과가 같다.

String의 `equals()` 는 String 클래스에서 오버라이드 된 메서드로 객체가 갖는 문자열 값을 비교하는 반면 이 메서드는 객체 자신의 참조값과 전달인자로 받는 객체의 참조값을 비교하는 차이가 있다.

#### equals의 실제 코드

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

### toString()

클래스명 + `@` + 해시코드를 16진수로 변환해 리턴한다.

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

### notify(), notifyAll()

이 객체를 사용하려고 대기중엔 쓰레드를 하나 혹은 모두 재개한다.

### finalize()

이 객체에 대한 참조가 더 이상 없다고 판단될 때(즉, 소멸될 때) 가비지 컬렉터에 의해 자동으로 호출되는 메서드

### getClass()

Class 인스턴스를 리턴한다. 이 인스턴스는 객체 자신의 클래스 정보를 갖는다.

### hashCode()

객체 자신의 해시코드 리턴

### wait( [long timeout] [, int nanos] )

쓰레드를 지정된 시간동안 기다리게 한다. timeout은 10^-3초, nanos는 10^-9초를 의미한다. 전달인자가 없으면 notify(), notifyAll()을 호출할 때까지 대기한다.
