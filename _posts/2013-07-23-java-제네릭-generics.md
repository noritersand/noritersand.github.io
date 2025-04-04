---
layout: post
date: 2013-07-23 21:21:00 +0900
title: '[Java] 제네릭 Generics'
categories:
  - java
tags:
  - java
  - generic
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Lesson: Generics (The Java™ Tutorials > Learning the Java Language)](https://docs.oracle.com/javase/tutorial/java/generics/index.html)

제네릭이란 클래스 혹은 메서드 내부에서 사용될 데이터 타입을 외부에서 지정하는 기법, 혹은 이러한 기법을 구현한 클래스와 메서드를 의미한다. 제네릭 클래스나 제네릭 메서드에선 제네릭 변수(혹은 `Type` 변수)를 사용하는데, 제네릭 변수는 `T` 키워드로 선언하며 데이터 타입은 컴파일 시점에 결정된다.

대표적인 제네릭 클래스로는 `Class` 클래스와 `List`, `Map`이 있다.


## 제네릭 클래스

제네릭 클래스는 제네릭 변수를 멤버로 가지는 클래스를 말하며, 제네릭 변수의 가짓수만큼 클래스명 다음 `<>` 안에 임의의 문자로 정의한다:

### 선언

```
class Generic< A [, B, C, ...] > {
    public A name;
    ...
}
```

```java
// 유형 1
class Gen1<Type> {
    public Type a;
}

// 유형 2
class Gen2<Type> {
    public Type a;
    public Type b;
    public Type c;
}

// 유형 3
class Gen3<Type1, Type2> {
    public Type1 a;
    public Type2 b;
}
```

제네릭 타입은 인스턴스가 생성될 때 어떤 타입인지 결정된다. 따라서 스태틱 컨텍스트에서는 제네릭을 사용할 수 없다:

```java
public static T num; // Cannot make a static reference to the non-static type T
```

### 초기화

초기화할 땐 반드시 데이터 타입을 명시하며 인스턴스를 생성해야 한다:

```java
Gen<Integer> iGen = new Gen<Integer>();
Gen<Object> oGen = new Gen<Object>();
```

다음은 생성자로 참조값을 전달하고 그 값을 다시 가져오는 클래스를 구현한 예다:

```java
class Generics<T> {
    public T parameter;

    public Generics() {
        //제네릭 클래스의 기본생성자
    }

    public Generics(T parameter) {
        this.parameter = parameter;
    }

    public T getParameter() {
        return parameter;
    }
}
```

```java
Generics<String> strGen = new Generics<String>("ㅎㅇ");
System.out.println(strGen.getParameter()); // "ㅎㅇ"

Generics<Integer> intGen = new Generics<Integer>(12);
System.out.println(intGen.getParameter()); // 12
```

제네릭 변수는 데이터 타입을 미리 알 수 없기 때문에 일반적인 연산은 할 수 없다:

```java
public T getData() {
    return parameter + parameter;
    // The operator + is undefined for the argument type(s) T, T
}
```


## 제네릭 메서드

제네릭 메서드는 매개변수의 타입을 제네릭 변수로 선언한 메서드를 말한다. 하나 이상의 타입에 대응하는 메서드를 작성해야 할 때 사용한다. 타입은 메서드 반환 타입 전 `<>` 안에서 정의한다. 반환 타입과 혼동하기 쉬우니 주의할 것.

```java
public class Generics {
    public static void main(String[] args) {
        getType(new Integer(0)); // class java.lang.Integer
        getType(new String()); // class java.lang.String
    }

    public static <T> String getType(T arg) {
        String clazz = arg.getClass().toString();
        return clazz;
    }
}
```

제네릭 메서드에서 `<T>`는 '로컬 제네릭 타입 파라미터'라고 하며, 해당 메서드 내에서만 유효하다. 클래스 수준에서 선언된 제네릭 타입과 별개다:

```java
public class WrongGenerics<T> {
    private T data;

    private <T> void setData(T data) {
        this.data = data; // error: incompatible types: T#1 cannot be converted to T#2
        // where T#1,T#2 are type-variables:
        //   T#1 extends Object declared in method <T#1>setData(T#1)
        //   T#2 extends Object declared in class Generics
    }
}
```



## 타입 제한 Bounded Type Parameters

제네릭 변수의 데이터 타입을 특정 서브타입이나 슈퍼타입으로 제한하는 기능이다. `super` 혹은 `extends` 키워드를 사용한다.

```
<T super ChildType>
<T extends ParentType>
```

제네릭 클래스의 경우 다음처럼 선언한다:

```java
class RestrictedGeneric<T extends BigDecimal> {
    private T value1;
}
```

위의 경우 제네릭 변수의 타입은 반드시 `BigDecimal`을 상속한 타입이어야 한다:

```java
RestrictedGeneric<Integer> a = new RestrictedGeneric<>(); // 컴파일 에러
// Bound mismatch: The type Integer is not a valid substitute for the bounded parameter <T extends BigDecimal> of the type RestrictedGeneric<T>
```

메서드의 경우, 아래처럼 선언하며:

```java
public CustomGeneric {
    public <T extends BigDecimal> void getSome(T arg) {
        ...
    }
}
```

해당 메서드의 전달인수가 `BigDecimal`의 서브타입이 아니면 컴파일 에러가 발생한다:

```java
CustomGeneric gen = new CustomGeneric();
gen.getSome("123"); // 컴파일 에러
// The method getSome(T) in the type GenericMethodTest.CustomGeneric is not applicable for the arguments (String)
```

### 제네릭 타입이 매개변수일 때의 타입 제한

`T`가 와일드카드`?`로 바뀌는 것 빼고 같다:

```java
private static List<String> getWeapons(List<? extends Equipment> weaponList) {
    ...
}
```

이 경우 `extends` 대신 `super`를 적용할 수도 있는데:

```java
public void getSome2(List<? super TinyDecimal> number) {
    ...
}
```

이러면 `TinyDecimal`의 슈퍼타입으로 제한하는 기능이 된다.
