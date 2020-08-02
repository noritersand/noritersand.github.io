---
layout: post
date: 2013-07-23 21:21:00 +0900
title: '[Java] 제네릭 generic'
categories:
  - java
tags:
  - java
  - generic
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://docs.oracle.com/javase/tutorial/java/generics/index.html](https://docs.oracle.com/javase/tutorial/java/generics/index.html)

제네릭이란 클래스 혹은 메서드 내부에서 사용될 데이터 타입을 외부에서 지정하는 기법, 혹은 이러한 기법을 구현한 클래스와 메서드를 의미한다. 제네릭 클래스나 제네릭 메서드에선 제네릭 변수(혹은 `Type` 변수)를 사용하는데, 제네릭 변수는 `T` 키워드로 선언하며 데이터 타입은 컴파일 시점에 결정된다.

대표적인 제네릭 클래스로는 `Class` 클래스와 `List`, `Map`이 있다.

## 제네릭 클래스

### 선언

```
class Generic<T> {
    public T num;
    public T num2;
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

오직 인스턴스 변수만이 제네릭 타입으로 선언될 수 있으며 스태틱이나 파이널 키워드는 사용할 수 없다:

```java
public static T num; // Cannot make a static reference to the non-static type T
public final T num = 0; // Type mismatch: cannot convert from int to T
public final static T num = 0; // Cannot make a static reference to the non-static type T
```

### 초기화

데이터 타입을 명시하면서 인스턴스를 생성한다:

```java
Gen<Integer> intGen = new Gen<Integer>();
Gen<Object> obGen = new Gen<Object>();
```

다음은 생성자로 참조값을 전달하고 그 값을 다시 가져오는 클래스를 구현한 예다:

```java
class GenericExample<T> {
    public T parameter;

    public GenericExample() {
        //제네릭 클래스의 기본생성자
    }

    public GenericExample(T parameter) {
        this.parameter = parameter;
    }

    public T getParameter() {
        return parameter;
    }
}

GenericExample<String> strGen = new GenericExample<String>("ㅎㅇ");
System.out.println(strGen.getParameter());
→ ㅎㅇ

GenericExample<Integer> intGen = new GenericExample<Integer>(12);
System.out.println(intGen.getParameter());
→ 12
```

제네릭 변수의 데이터 타입을 알 수 없기 때문에 내부에서 일반적인 연산은 불가능하다:

```java
public T getData() {
    return parameter + parameter;
    // The operator + is undefined for the argument type(s) T, T
}
```

## 제네릭 메서드

제네릭 메서드는 매개변수의 타입을 제네릭 변수로 선언한 메서드를 말한다. 하나 이상의 타입에 대응하는 메서드를 작성해야 할 때 사용한다.

```java
public class GenericExample {
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

## 타입 제한

제네릭 변수의 데이터 타입을 특정 서브타입으로 제한하는 기능이다. `extends` 키워드와 함께 슈퍼타입을 명시하는 방식으로 사용한다.

```
<T extends ParentType>
```

예를 들어, 아래 작성한 `CustomGeneric에서` 제네릭 변수의 데이터 타입은 반드시 `java.math.BigDecimal`의 서브타입이어야 한다:

```java
public class GenericClassTest {
    @Test
    public void test() {
        CustomGeneric<BigDecimal> gen = new CustomGeneric<>(); // correct
        CustomGeneric<LittleDecimal> gen2 = new CustomGeneric<>(); // correct
        CustomGeneric<Integer> gen3 = new CustomGeneric<>(); // wrong
        // Bound mismatch: The type Integer is not a valid substitute for the bounded parameter <T extends BigDecimal> of the type CustomGeneric<T>
    }

    private class CustomGeneric<T extends BigDecimal> {
        // T는 BigDecimal의 자식 클래스만 올 수 있음
    }

    private class LittleDecimal extends BigDecimal {
        private static final long serialVersionUID = 2718457985045593298L;
        public LittleDecimal(int val) {
            super(val);
        }
    }
}
```

```java
public class GenericMethodTest {
    @Test
    public void test() {
        CustomGeneric gen = new CustomGeneric();
        gen.getSome(new LittleDecimal(123)); // correct
        gen.getSome("123"); // wrong
        // The method getSome(T) in the type GenericMethodTest.CustomGeneric is not applicable for the arguments (String)
        gen.getSome(123); // wrong
        // The method getSome(T) in the type GenericMethodTest.CustomGeneric is not applicable for the arguments (int)
    }

    private class CustomGeneric {
        public <T extends BigDecimal> void getSome(T number) {
            System.out.println(number);
        }
    }

    private class LittleDecimal extends BigDecimal {
        private static final long serialVersionUID = 2718457985045593298L;
        public LittleDecimal(int val) {
            super(val);
        }
    }
}
```

### 컬렉션이나 맵일 때 제네릭 타입 제한

`T` 키워드가 `?`로 바뀌는 것 빼고 같다.

```java
    private static List<String> getWeapons(List<? extends Equipment> weaponList) {
        if (CollectionUtils.isEmpty(weaponList)) {
            return null;
        }
        List<String> nums = new LinkedList<>();
        for (Equipment ele : weaponList) {
            nums.add(ele.getName());
        }
        return intgEventIds;
    }
```
