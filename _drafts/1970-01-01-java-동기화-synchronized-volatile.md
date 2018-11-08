---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Java: 동기화 synchronized, volatile'
categories:
  - java
tags:
  - java
  - synchronized
  - volatile
---

#### 관련 문서
- [https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html)
- [https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html](https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html)
- [https://docs.oracle.com/javase/tutorial/essential/concurrency/atomic.html](https://docs.oracle.com/javase/tutorial/essential/concurrency/atomic.html)
- [http://thswave.github.io/java/2015/03/08/java-volatile.html](http://thswave.github.io/java/2015/03/08/java-volatile.html)
- [http://blog.javarouka.me/2012/04/volatile-keyword-in-java.html](http://blog.javarouka.me/2012/04/volatile-keyword-in-java.html)

## synchronized
synchronized 키워드는 메서드나 메서드 내부의 특정 지역을 동기화 처리할 때 사용한다.
여기서 동기화란 다중 스레드 환경에서 특정범위를 한 번에 한 스레드만 접근하도록 제한함을 의미한다.
```java
public class LogicTest {
    ...

    public synchronized void synchronizedMethod() {
        // 메서드에 선언
    }

    public void synchronizedStatement() {
        synchronized (this) {
            // 블록으로 선언
        }
    }
}
```
synchronized 블록의 표현식은 동기화의 기준이 된다. 여기서 기준이란 caller에 따라 동기화를 할 지 말지를 결정함을 의미한다. this나 Object.class로 작성할 경우 (아마도)VM 기준, 모든 caller가 동기화 대상이 된다. TODO: 확인할 것

synchronized 메서드는 synchronized 블럭의 표현식이 this인 것과 같다. 인스턴스를 기준으로 동기화된다.

## volatile
TODO: 이건 뭐지

아래부터 퍼온 내용 (출처: http://egloos.zum.com/iilii/v/4071694)

---
요즘 jsr 133 자바 메모리 모델을 보고 있는데, 처음에 lock거는 부분에 대해 나오더군요.
자바에서 lock을 거는 것은 보통 synchronized 구문을 이용하는데, 일반적인 자바 책에서는 "synchronized를 걸면 동시 접속이 안된다." 까지만 나와 있었는데, 보다가 보니 재미있는 부분들이 있어서 정리해볼랍니다.
```java
synchronized (anObject){
    // code!!
}
```
위와 같은 구문은 anObject를 기준으로 잡습니다. 그렇기 때문에 완전히 다른 객체끼리도 동기화가 가능합니다. 다음은 제가 주로 사용하는 디버깅 코드입니다.
```java
synchronized (java.lang.Object.class) {
    System.out.println("===========디버깅 시작했다~================");
    System.out.println("time:"
            + new java.text.SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new java.util.Date()));
    System.out.print(new Throwable().getStackTrace()[0].getClassName() + "."
            + new Throwable().getStackTrace()[0].getMethodName() + "()");
    System.out.println("  line: " + new Throwable().getStackTrace()[0].getLineNumber());
    System.out.println(something);
    System.out.println("===========디버깅 끝났다~================");
}
```
`java.lang.Object`는 하나이므로 `java.lang.Object`를 기준으로 동기화를 시키면 저 코드가 어디 들어가 있더라도 저 코드들끼리 꼬이는 일은 없습니다. 한번 =======디버깅 시작했다.========= 구문이 찍히면, ======== 디버깅 끝났다.===== 구문이 찍히기 전까지는 다른 쓰레드에서 저런 디버깅 메시지를 찍지 않고 기다리게됩니다.
메쏘드에 선언된 synchronized는 자기 객체(this)를 기준으로 동기화합니다.(static 메쏘드의 경우는 자기 클래스를 기준으로!) 그 객체에서 synchronized를 건 모든 메쏘드끼리도 동기화가 됩니다.
다음 코드를 보세요.
```java
package synch.method1;

public class BlackOrWhite {
    private String str;
    private final String BLACK = "black";
    private final String WHITE = "white";

    public synchronized void black(){
        str = BLACK;
        try {
            long sleep = (long) (Math.random()*100L);
            Thread.sleep(sleep);
            if (!str.equals(BLACK)) {
                System.out.println("+++++++++++++++++broken!!++++++++++++++++++++");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    public synchronized void white(){
        str = WHITE;
        try {
            long sleep = (long) (Math.random()*100L);
            Thread.sleep(sleep);
            if (!str.equals(WHITE)) {
                System.out.println("+++++++++++++++++broken!!++++++++++++++++++++");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
```java
package synch.method1;

import java.util.Iterator;

public class Test {

    /**
     * @param args
     */
    public static void main(String[] args) {
        final BlackOrWhite bow = new BlackOrWhite();

        Thread white = new Thread() {
            public void run() {
                while (true) {
                    bow.white();
                }
            }
        };
        Thread black = new Thread() {
            public void run() {
                while (true) {
                    bow.black();
                }
            }
        };
        white.start();
        black.start();
    }
}
```
두개의 쓰레드가 BlackOrWhite 클래스의 인스턴스인 bow라는 변수를 공유하고 있습니다. 하나의 쓰레드는 black()이란 메쏘드만 주구장창 호출하고, 다른 하나의 쓰레드는 white()만 주구장창 호출해댑니다. 그런데, black()이란 메쏘드와 white()라는 메쏘드가 서로 동기화가 됩니다. 위 코드를 실행시키면 broken!이 찍히는 일은 절대 없습니다. 다시 말해 서로 다른 쓰레드에서 black()과 white()를 호출하지만 두 메쏘드 모두 bow라는 인스턴스에 소속되기 때문에 두 메쏘드가 동시에 진행되는 일은 일어나지 않습니다.
정리를 하자면.
```java
synchronized foo(){
    //메쏘드 내용.
}
```
과
```java
foo(){
    synchronized(this){
        //내용
    }
}
```
은 같은 얘기가 됩니다.

경우에 따라서 method 간에 동기화가 필요 없는 경우도 있을 수 있습니다. 다음의 예를 봅시다.
```java
package synch.two;

import java.util.HashMap;
import java.util.Map;

public class TwoMap {
    private Map<String, String> map1 = new HashMap<String, String>();
    private Map<String, String> map2 = new HashMap<String, String>();

    public synchronized void put1(String key, String value){
        map1.put(key, value);
    }
    public synchronized void put2(String key, String value){
        map2.put(key, value);
    }

    public synchronized String get1(String key){
        return map1.get(key);
    }
    public synchronized String get2(String key){
        return map2.get(key);
    }
}
```
위의 예제에서는 두개의 map을 가진 객체가 있습니다. 두 map에 각각 put과 get을 지원합니다. put1()과 get1()은 동기화가 되어야 하지만, put1()과 put2()가 동기화될 필요는 없습니다. 그러나 method에 synchronized가 걸려 있기 때문에 4개의 메쏘드 모두 상호 동기화가 됩니다. 그다지 바람직하지 않은 코드죠.
위의 코드는 아래와 같이 바뀌는 것이 좋습니다.
```java
package synch.two;

import java.util.HashMap;
import java.util.Map;

public class TwoMap {
    private Map<String, String> map1 = new HashMap<String, String>();
    private Map<String, String> map2 = new HashMap<String, String>();
    private final Object syncObj1 = new Object();
    private final Object syncObj2 = new Object();

    public void put1(String key, String value){
        synchronized (syncObj1) {
            map1.put(key, value);
        }
    }
    public void put2(String key, String value){
        synchronized (syncObj2) {
            map2.put(key, value);
        }
    }

    public String get1(String key){
        synchronized (syncObj1) {
            return map1.get(key);
        }
    }
    public String get2(String key){
        synchronized (syncObj2) {
            return map2.get(key);
        }
    }
}
```
이렇게 하면 put1()과 get1() 끼리만, put2()와 get2()끼리만 동기화가 됩니다. 위의 예제에서는 굳이 syncObj1, syncObj2를 만들지 않고, map1과 map2를 이용하여 동기화하여도 됩니다만, 동기화를 위해 저렇게 동기화 전용 멤버 변수를 만들 수도 있다는 것을 보여드릴라고 한 겁니다.

---
여기까지
