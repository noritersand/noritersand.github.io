---
layout: post
date: 2016-11-02 10:11:00 +0900
title: 'Code convention: 줄 나누기'
categories:
  - etc
tags:
  - code convention
  - coding guide
---

* Kramdown table of contents
{:toc .toc}

줄 나누기
하나의 식이 한 줄에 들어가지 않을 때에는, 다음과 같은 일반적인 원칙들을 따라서 두 줄로 나눈다.

콤마 후에 두 줄로 나눈다.
연산자(operator) 앞에서 두 줄로 나눈다.
레벨이 낮은 원칙 보다는 레벨이 높은 원칙에 따라 두 줄로 나눈다.
앞줄과 같은 레벨의 식(expression)이 시작되는 새로운 줄은 앞줄과 들여쓰기를 일치시킨다.
만약 위의 원칙들이 코드를 더 복잡하게 하거나 오른쪽 끝을 넘어간다면, 대신에 8개의 빈 칸을 사용해 들여쓴다.


#### 메서드 호출을 두 줄로 나누어 쓸 때

```java
someMethod(longExpression1, longExpression2, longExpression3,
        longExpression4, longExpression5);

String a = someMethod1(longExpression1,
        someMethod2(longExpression2,
                longExpression3));
```

#### 수학 표현식을 두 줄로 나누어 작성할 때

```java
// 괄호로 싸여진 표현식 밖에서 줄 바꿈이 일어나고 더 높은 레벨이기 때문에 될 수 있으면 이 방법을 사용
longName1 = longName2 * (longName3 + longName4 - longName5)
        + 4 * longname6;

// 될 수 있으면 피하는방법
longName1 = longName2 * (longName3 + longName4
        - longName5) + 4 * longname6;
```

#### 메서드 선언을 들여쓸 때

```java
// 일반적인 들여쓰기
someMethod(int anArg, Object anotherArg, String yetAnotherArg,
        Object andStillAnother) {
    ...
}

// 너무 멀리 들여쓰는 것을 피하기 위해 8개의 빈 칸으로 들여쓰기
private static synchronized horkingLongMethodName(int anArg,
        Object anotherArg, String yetAnotherArg,
        Object andStillAnother) {
    ...
}
```

일반적으로 메서드 본문이 시작할 때 4개의 빈 칸을 사용. 메서드 본문과 구분하기 위해서 줄을 나누는 경우의 들여쓰기는 일반적으로 8개의 빈 칸 원칙을 사용한다.

```java
// 아래와 같은 들여쓰기는 사용하지 않는 것이 좋다.
if ((condition1 && condition2)
    || (condition3 && condition4)
    ||!(condition5 && condition6)) { // 좋지 않은 들여쓰기
    doSomethingAboutIt(); // 메서드 본문 시작이 명확하지가 않다.
}

// 대신에 아래와 같은 들여쓰기를 사용한다.
if ((condition1 && condition2)
        || (condition3 && condition4)
        ||!(condition5 && condition6)) {
    doSomethingAboutIt();
}

// 또는 아래와 같은 들여쓰기를 사용한다.
if ((condition1 && condition2) || (condition3 && condition4)
        ||!(condition5 && condition6)) {
    doSomethingAboutIt();
}
```

#### 삼항식(ternary expression)을 사용할 때

```java
alpha = (aLongBooleanExpression) ? beta : gamma;

alpha = (aLongBooleanExpression) ? beta
        : gamma;

alpha = (aLongBooleanExpression)
        ? beta
        : gamma;
```
