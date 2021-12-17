---
layout: post
date: 2014-12-12 11:40:00 +0900
title: '[Spring] AOP annotation'
categories:
  - spring
tags:
  - java
  - spring
  - aop
---

* Kramdown table of contents
{:toc .toc}

Spring-AOP 관련 annotation

관련 용어
advice : 공통기능을 구현한 클래스
target : advice가 적용될 객체
joinpoint : advice가 적용될 지점(target의 메서드)
pointcut : 실제로 advice가 적용된 joinpoint (pointcut은 joinpoint의 부분집합)
advisor(= aspect) : advice + pointcut
weaving : advice를 핵심 로직 코드에 적용하는 것

@Aspect
개요 : AspectJ 5 버전에 새롭게 추가된 어노테이션으로서 어스팩트 어노테이션을 사용하면 설정파일에 Advice 및 Pointcut 등의 설정을 하지 않고도 자동으로 Advice 를 적용할수 있다.
      설정 위치: 클래스 선언부 위
      추가설정 : XML 설정파일에 <aop:aspectj-autoproxy />를 정의 한다.
                    <aop:aspectj-autoproxy /> 태그는 @Aspect 어노테이션이 적용된 클래스를 로딩하여 클래스에 명시된 Advice 및 Pointcut 정보를 이용하여 알맞은 빈 객체에 Advice 를 적용하게 된다.

@Aspect 어노테이션이 적용된 클래스
Advice로 사용될 메서드에는 Advice를 의미하는 어노테이션을 설정 한다.

1) @Before 어노테이션
Before Advice로 사용할 메서드에 적용.
2) @After 어노테이션
After Advice로 사용할 메서드에 적용.
3) @AfterReturning 어노테이션과 @AfterThrowing 어노테이션
각각 반환값과 예외 객체를 전달받을 파라미터의 이름을 지정하며 Advice 메서드에서 반환값과 예외 객체를 사용할 수 있도록 한다.
4) @Around 어노테이션을 제외한 나머지 Advice를 표시하는 어노테이션들
메서드는 프록시 대상 객체에 대한 정보가 필요한 경우 첫 번째 파라미터로 JoinPoint를 사용.
5) @Around 어노테이션 사용
ProceedingJoinPoint.proceed() 메서드를 호출하여 프록시 대상 객체의 메서드를 호출한다.

   ο AspectJ의 Pointcut 표현식
     AspectJ는 Pointcut을 명시할 수 있는 다양한 명시자를 제공하는데, 스프링은 메서드 호출과 관련된 명시자만을 지원하고 있다. execution 명시자는 Advice를 적용할 메서드를 명시할 때 사용되며, 기본 형식은 다음과 같다.

     execution(수식어_패턴 리턴타입_패턴 패키지_패턴.클래스명_패턴.메서드명_패턴(파라미터_패턴)
      - 수식어_패턴 : 생략 가능하며 public, protected 등이 온다.
      - 리턴타입_패턴 : 리턴 타입을 명시 한다.
      - 패키지_패턴, 클래스명_패턴, 메서드명_패턴 : 클래스 이름 및 메서드 이름을 패턴으로 명시 한다.
      - 파라미터_패턴 : 매칭될 파라미터에 대하여 명시 한다.

     각 패턴은 『*』을 이용하여 모든 값을 표현할 수 있으며, 『..』을 이용하여 0개 이상이라는 의미를 표현 할 수 있다.
    within 명시자는 메서드가 아닌 특정 타입에 속하는 메서드를 Pointcut으로 설정할 때 사용된다.
    각각의 표현식은 『&&』 나 『||』 연산자를 이용하여 연결할 수 있다. 예를 들어, @Aspect 어노테이션을 이용하는 경우,

    @AfterThrowing(pointcut="execution(public * get*()) && execution(public void set*(..))")
    public void throwingLogging() {
       ...
    }
