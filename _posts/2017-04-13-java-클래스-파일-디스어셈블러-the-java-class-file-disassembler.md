---
layout: post
date: 2017-04-13 11:09:00 +0900
title: '[Java] 클래스 파일 디스어셈블러 The Java Class File Disassembler'
categories:
  - java
tags:
  - java
  - disassembler
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://docs.oracle.com/javase/8/docs/technotes/tools/windows/javap.html](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/javap.html)

클래스 파일의 바이트 코드를 확인할 수 있는 명령어. 컴파일 후 사용한다.

#### syntax

```
javap -c 클래스명
```

#### example

```bash
> javac TestClass.java
> javap -c TestClass

Compiled from "TestClass.java"
public class TestClass {
  public TestClass();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public java.lang.String concatTest1();
    Code:
       0: ldc           #2                  // String 111222333
       2: astore_1
       3: aload_1
       4: areturn

  public java.lang.String concatTest2();
    Code:
       0: ldc           #3                  // String 444
       2: astore_1
       3: new           #4                  // class java/lang/StringBuilder
       6: dup
       7: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
      10: aload_1
      11: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      14: ldc           #7                  // String 555
      16: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      19: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      22: astore_1
      23: new           #4                  // class java/lang/StringBuilder
      26: dup
      27: invokespecial #5                  // Method java/lang/StringBuilder."<init>":()V
      30: aload_1
      31: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      34: ldc           #9                  // String 666
      36: invokevirtual #6                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      39: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      42: astore_1
      43: aload_1
      44: areturn

  public java.lang.String concatTest3();
    Code:
       0: ldc           #10                 // String 777
       2: ldc           #11                 // String 888
       4: invokevirtual #12                 // Method java/lang/String.concat:(Ljava/lang/String;)Ljava/lang/String;
       7: ldc           #13                 // String 999
       9: invokevirtual #12                 // Method java/lang/String.concat:(Ljava/lang/String;)Ljava/lang/String;
      12: astore_1
      13: aload_1
      14: areturn
}
```

여기서 사용한 TestClass는 요렇게 생겼다:

```java
TestClass
public class TestClass {

    public String concatTest1() {
        String str = "111" + "222" + "333";
        return str;
    }

    public String concatTest2() {
        String str = "444";
        str += "555";
        str += "666";
        return str;
    }

    public String concatTest3() {
        String str = "777".concat("888").concat("999");
        return str;
    }
}
```

#### 참고

JDK 1.8인가 1.7인가 부터 문자열 + 연산을 컴파일러가 알아서 StringBuilder를 사용하도록 바꿔줌.
