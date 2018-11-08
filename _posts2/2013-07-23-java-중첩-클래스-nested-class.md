---
layout: post
date: 2013-07-23 23:14:00 +0900
title: 'Java: 중첩 클래스 nested class'
categories:
  - java
tags:
  - java
  - nested
  - anonymous
  - interface
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)
- [https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html)
- [http://blog.naver.com/PostView.nhn?blogId=dethgray&logNo=80087298541](http://blog.naver.com/PostView.nhn?blogId=dethgray&logNo=80087298541)
- [http://blog.saltfactory.net/191](http://blog.saltfactory.net/191)

중첩 클래스란 클래스 내부에 또 다른 클래스를 선언하는 기법을 말한다. 클래스를 클래스의 멤버처럼 취급하며 프로그램의 구조를 간략화하는 특징이 있다. 제한적인 지역에서만 필요한 클래스를 선언할 때 사용한다. 내부 클래스라고도 한다.

**이 글에서 내부 클래스란 nested class 자신을, 외부 클래스란 nested class를 소유한 클래스를 의미함.**

중첩 클래스는 외부 클래스의 클래스명에 달러표시`$`와 자기 자신의 클래스명이 붙은 형태로 컴파일된다. 예를 들어 Parent 클래스 내에 Child라는 중첩클래스가 있으면 컴파일 후 Parent$Child.class 라는 파일이 생성된다. 만약 중첩 클래스가 익명일 경우엔 선언된 순서에 따라 숫자로 대체된다(Parent.java의 맨 위에서부터 첫 번째 익명 클래스는 Parent$1.class, 두 번째 익명 클래스는 Parent$2.class 이런 식).

## static nested class

클래스 내부에 static으로 선언된 중첩 클래스를 말한다. 스태틱 중첩 클래스는 외부 클래스에서 마치 독립된 클래스를 다루듯이 접근할 수 있다.

```java
class Outer {
    private static int outerMember;

    public static class Nested {
        private static int innerMember;

        public static void nestedM() {
            System.out.println(outerMember + innerMember);
        }
    }

    public static void outerMethod() {
        System.out.println(outerMember + Nested.innerMember);
              // 외부메서드에서도 내부 클래스의 멤버에 접근할 수 있다.
              // 같은 클래스로 간주하기 때문에 private은 무시된다.
    }
}
```

다른 클래스에서 접근하는 방법은:

```java
Outer.Nested.nestedM();
Outer.Nested.innerMember; //private이기 때문에 접근불가
```

## (non-static)nested class

스태틱 중첩 클래스와 비교하면 단순히 static만 빠진 형태. 스태틱 멤버를 선언할 수 없다. 외부에서 접근하려면 인스턴스를 생성해야 한다.

```java
class Outer {
	private static String a = "static outer member";
	private String b = "non-static outer member";

	private class Inner {
//		private static String c = "static inner member"; // non-static 내부 클래스에는 스태틱 멤버를 선언할 수 없음
		private String d = "non-static inner member";

		private void innerMethod() {
			System.out.println(Outer.a); // private이어도 접근 가능
			Outer out = new Outer();
			System.out.println(out.b); // private이어도 접근 가능
		}
	}

	public void outerMethod() {
		Inner inner = new Inner();
		System.out.println(inner.d); // private이어도 접근 가능
		inner.innerMethod();
	}
}
```

다른 클래스에선 내부클래스에 직접 접근할 수 없으므로 우선 외부클래스의 인스턴스를 만들고 외부클래스의 getter를 통해 내부클래스의 인스턴스를 얻어야 한다. (물론 이런 getter가 제공될리가 없겠지만)

```java
import org.junit.Assert;
import org.junit.Test;

public class NestedTest {
	@Test
	public void test() {
		Outer outer = new Outer();
		Assert.assertEquals("invoking innerMethod", outer.outerMethod());

		Outer.Inner inner = new Outer().getInner();
		Assert.assertEquals("invoking innerMethod", inner.innerMethod());
	}
}

class Outer {
	public Inner getInner() {
		return new Inner();
	}

	public class Inner {
		public String innerMethod() {
			return "invoking innerMethod";
		}
	}

	public String outerMethod() {
		return new Inner().innerMethod();
	}
}
```

### example: nested enum type

```java
public class NestedEnumTest {
	@Test
	public void test() {
		NestedEnumTestBean.InsertType insertType = NestedEnumTestBean.InsertType.APPEND;
		Assert.assertEquals("APPEND", insertType.toString());
	}
}

class NestedEnumTestBean {
	private InsertType emailInsertType;

	public enum InsertType {
		APPEND, NEW;
	}

	public InsertType getEmailInsertType() {
		return emailInsertType;
	}

	public void setEmailInsertType(InsertType emailInsertType) {
		this.emailInsertType = emailInsertType;
	}
}
```

## local class

메서드 블록 내에서 선언한 클래스로, 일회성 클래스가 필요할 때 사용한다.

```java
class Outer {
    public void outerM() {
        int localMember = 30;

        class Local {
            private int d = 40; //static 생성불가

            public void localM() {
                // ...
            }
        }
    }
}
```

로컬 클래스는 선언한 메서드에서만, 위와 같은 경우엔 `outerM()` 메서드 안에서만 사용할 수 있다. 인스턴스 생성은 불가능하며 static을 사용할 수 없다.

## anonymous class

익명 클래스. **이름이 없는 로컬 클래스다.** 익명 클래스는 반드시 어떠한 클래스를 상속받거나 인터페이스의 구현체 형태여야 한다.

```java
class Outer {
    public Object outerM() {

        return new Object() {
            // 익명 클래스 영역 -> 멤버선언가능

            @Override
            public String toString() {
                return "오버라이딩";
            }
        };
    }
}

Outer o = new Outer();
System.out.print(o.outerM());

→ 오버라이딩
```

인터페이스 구현체를 익명 클래스로 작성하는 방식은 다음과 같다:

```java
interface Inner {
    void printMessage();
}

public class Outer {
    public static void main(String[] args) {
        Inner inner = new Inner() {
            @Override
            public void printMessage() {
                System.out.println("overrided");
            }
        };
        inner.printMessage();
    }
}

-> "overrided"
```

익명 클래스를 활용하면 메서드의 내용을 호출부(caller)에서 작성하고 피호출부(callee)에서 해당 메서드를 실행하는 콜백(callback) 구조를 만들 수 있다.

```java
interface Callback {
    public void outPrint(String msg);

    public void doSomethingWithCaller();
}

class Callee {
    public static void sayHello(Callback callback, String msg) {
        callback.outPrint(msg);
        callback.doSomethingWithCaller();
    }
}

public class Caller {
    public static void main(String[] args) {

        final String phone = "010-xxx1-4xxx";
        // 내부 클래스에서 참조하는 지역변수는 반드시 파이널 변수여야 함.

        Callback callback = new Callback() {
            @Override
            public void outPrint(String msg) {
                System.out.println(msg);
            }

            @Override
            public void doSomethingWithCaller() {
                System.out.println("my number is " + phone);
            }
        };

        Callee.sayHello(callback, "Hello there! Mighty fine morning! if you ask me, I'm Waldo.");
    }
}
```

### example: callback method

```java
class Sum {
    interface OnMaxNumberCb {
        void onMaxNumber(int number, int exceed);
    }

    private int number = 0;
    private int maxNumber = 0;
    private OnMaxNumberCb myCallback;

    public void setOnMaxNumberCb(OnMaxNumberCb callback) {              
        myCallback = callback;
    }

    public void setMaxNumber(int max) {
        maxNumber = max;
    }

    public int addNumber(int adder) {
        number = number + adder;

        if (myCallback != null) {
            if (number >= maxNumber) {
                myCallback.onMaxNumber(number, number - maxNumber);
            }
        }

        return number;
    }

    public int getTotal() {
        return number;
    }
}


public class CallbackTest {
    public static void main(String[] args) {
        Sum total = new Sum();

        Sum.OnMaxNumberCb callback = new Sum.OnMaxNumberCb() {

            @Override
            public void onMaxNumber(int number, int exceed) {
                System.out.println("Current sum is " + number
                                    + " and exceeds" + exceed);
            }
        };

        total.setMaxNumber(50);
        total.setOnMaxNumberCb(callback);

        for(int i=1; i<=11; i++) {
            total.addNumber(i);
        }

        System.out.println("Total is " + total.getTotal());
    }
}
```
