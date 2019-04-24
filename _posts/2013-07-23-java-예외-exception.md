---
layout: post
date: 2013-07-23 21:38:00 +0900
title: 'Java: 예외 exception'
categories:
  - java
tags:
  - java
  - exception
  - try-catch
  - finally
---

* Kramdown table of contents
{:toc .toc}

Exception이란 런타임에서 발생하는 일종의 의도적으로 만들어낸 에러라고 볼 수 있다. (사용자 정의 에러)

참고: [differences-between-exception-and-error](https://stackoverflow.com/questions/912334/differences-between-exception-and-error)

API에 따라 예외를 내부에서 알아서 처리(`try-catch`)하거나 선언부에서 던지기도(`throws`) 하는데, 선언부에서 던지는 경우 호출부에서 예외처리(`try-catch` 혹은 `throws`)를 명시해야 한다. 단, 이 규칙은 `RuntimeException`이나 파생 타입일 땐 적용하지 않는다.

예외를 미루는 대표적인 API로는:

- 네트워크 통신
- 데이터베이스 처리
- 메모리 & 파일 입출력

가 있다.

## RuntimeException

JVM 실행 중에 발생하는 예외를 의미한다. JDK의 자바독을 보면 다음처럼 명시되어 있다.

>RuntimeException and its subclasses are uncheckedexceptions. Unchecked exceptions do not need to bedeclared in a method or constructor's throws clause if theycan be thrown by the execution of the method or constructor andpropagate outside the method or constructor boundary.

~~뭐라는거야~~ 대충 해석해보면 Unchecked exception으로 분류되며 이 유형은 메서드나 생성자에 의해 상위 스택으로 전파가 가능하다면 예외 처리를 명시하지 않아도 된다고 한다. ~~아님 말고~~

## try-catch

```
try {
   비즈니스 코드 블록
} catch(Exception e) {
   예외 처리 코드 블록, 예외 발생 시에만 실행
} finally {
   try, catch 뒤에 오며 예외 발생 여부와 관계없이 무조건 실행된다.
}
```

```java
try {
    throw new Exception("내가 만든 예외"); //Exception 강제 발생
} catch (Exception e) {
    //try절에서 던진(발생한) 예외를 처리하는 블록
    System.out.println(e.getMessage()); // 에러 메시지 출력
    e.printStackTrace(); // 에러 발생 시점의 호출 스택 출력
}
```

`ArrayIndexOutOfBoundsException`의 경우 배열의 범위를 초과할 때 발생하는 예외인데 아래처럼 코드작성 시 해당 예외가 발생할 것을 예측하여 작성할 수 있다:

```java
int[] nums = new int[] {10, 20, 30, 40, 50};

System.out.print("보고싶은 인덱스 : ");
int index = scan.nextInt();

try {
    System.out.println(nums[index]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println(e); // 발생한 예외의 메시지를 콘솔에 출력
    System.out.println("올바른 색인번호를 입력하세요.");
}
```

모든 경우의 수를 예측하는 것은 불가능하기 때문에 비효율적이긴 해도 다음처럼 모든 예외를 처리하는 방식으로 작성하기도 한다(권장하지 않음):

```java
try {
    // ...
} catch (Exception e) {
    // 예외처리 코드
}
```

## throw / throws

`throw`, `throws` 키워드는 예외가 발생한 지역이 아니라 예외를 발생시킨 메서드를 호출한 지역, 즉 caller에게 예외를 떠넘길 때 사용한다. 다음은 `m01()` 메서드에서 발생한 예외를 caller인 `main()`에서 처리하는 예다:

```java
public class Test {
    public static void main(String[] args) {
        Excep ex = new Excep();
        try {
            ex.m01();
        } catch (Exception e) {
            System.out.println("메서드를 호출한 지점");
        }
    }
}

class Excep {
    public void m01() {
        int[] nums = new int[] { 1 };
        try {
            System.out.println(nums[2]);
            // ArrayIndexOutOfBoundsException 발생
            // new ArrayIndexOutOfBoundsException(); : 1. 예외 객체 생성
            // throws new ArrayIndexOutOfBoundsException : 2. 예외객체를 던진다.
        } catch (Exception e) {
            throw e; // 이 메서드를 호출한 지역이 예외를 받는다
        }
    }
}
```

아래처럼 모든 메서드에서 예외를 미루는 방법도 있다:

```java
class Excep {
    public void m01() throws Exception {
        int[] nums = new int[] {1};
        System.out.println(nums[2]);
    }
}
```

## finally

`finally`는 try절이나 catch절 이후 반드시 실행되는 실행문을 정의한다.

```java
    @Test
    public void finallyTest2() {
        String str = "";
        try {
            str += "try";
            throw new IllegalAccessError();
        } catch (IllegalAccessError e) {
            str += " catch";
        } finally {
            str += " finally";
        }
        Assert.assertEquals("try catch finally", str);
    }
```

`try-catch-finally`에서 catch절 없이 `try-finally` 형태로 작성이 가능하긴 하다. 이 경우 try절에서 예외 발생 시 예외가 미뤄진다:

```java
    public void withOutCatchStatement() {
        try {
            try {
                @SuppressWarnings("unused")
                int nan = 1 / 0; // 여기서 발생한 예외는 가장 바깥의 catch에서 받음
            } finally {
                // catch문이 없으면 finally라도 있어야 컴파일 에러 안남
                logger.debug("You know nothing John snow!");
            }            
        } catch (ArithmeticException e) {
            logger.error(e.getMessage(), e);
        }
    }
```

## example

```java
try {
    int[] nums = { 10, 20, 30 };
    System.out.println(nums[5]); // ArrayIndexOutOfBoundsException

    Random rnd = null;
    System.out.println(rnd.nextInt()); // NullPointerException

} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("인덱스범위에러");

} catch (NullPointerException e) {
    System.out.println("null에러");
}
// 또 다른 예외 발생?
catch (Exception e) {
    // 던진 객체를 받을 수 있는 클래스가 없다면 부모 클래스인 Exception에 업캐스팅해서 받는다.
    System.out.println("어쨋든 에러");
}
```

```java
public class Ex94 {
    public static void main(String[] args) {

        //예외 코드
        // A001 : 배송 중 제품 파손
        // A002 : 배송 중 날짜 지연
        // A003 : 배송 중 잘못 배송
        // B001 : 사용 중 제품 불량
        // B002 : 사용 중 제품 오작동

        System.out.println("고객이 제품을 주문..");

        System.out.println("1. 주문접수 완료");
        System.out.println("2. 제품 포장");
        System.out.println("3. 제품 발송");

        System.out.println("고객이 제품을 수령..");


        try {
            System.out.println("제품파손 발견"); //A001

            throw new MyShopException("A001");

        } catch (MyShopException e) {

            System.out.println("예외 코드 : " + e);
            System.out.println("내선 번호 : " + e.checkNumber());
        }
    }
}

//사용자 정의 예외 클래스(Exception 파생 클래스)
class MyShopException extends Exception {
    //예외 코드 관리-> 담당 상담원 안내
    private String code; //예외코드

    MyShopException(String code) {
        this.code = code;
    }

    //내선번호
    public String checkNumber() {
        String number = "";
        if (this.code.equals("A001")) number = "100";
        else if (this.code.equals("A002")) number = "110";
        else if (this.code.equals("A003")) number = "120";
        else if (this.code.equals("B001")) number = "200";
        else if (this.code.equals("B002")) number = "210";

        return number; //생성자로 받아온 Exception code를 비교해 해당 문자열을 리턴하는 메서드
    }
}
```
