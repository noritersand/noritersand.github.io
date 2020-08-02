---
layout: post
date: 2019-05-03 14:08:00 +0900
title: '[Spring] SpEL, Spring Expression Language'
categories:
  - spring
tags:
  - java
  - spring
  - spel
---

#### 참고한 사이트와 문서

- [http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html)
- [https://blog.outsider.ne.kr/835](https://blog.outsider.ne.kr/835)

## 스프링 빈 접근자(#)

```bash
#{ beanId }
#{ beanId.propertyName }
#{ beanId[ 'propertyName' ] }
```

**TODO**: `#{systemProperties['PROPERTY_NAME']}`, `#{systemEnvironment['ENV_VARIABLE_NAME']}` 둘의 차이 확인할 것.

#### example\#1

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <entry key="some.test.prop">hi hello</entry>
</properties>
```

```xml
<util:properties id="testProperties" location="classpath:/config/test-properties.xml" />
```

```java
@Resource(name = "testProperties")
private Properties testProperties;

// 생략

String someProp = testProperties.getProperty("some.test.prop");
```

#### example\#2

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <entry key="web.root">file://C:/dev/workspace/laboratory/src/main/webapp</entry>
</properties>
```

```xml
<util:properties id="storageProp" location="classpath:/config/storage.xml" />
```

```xml
<util:map id="fileResourceMap" key-type="java.lang.String" value-type="org.springframework.core.io.Resource">
    <entry key="webRoot" value="#{storageProp['web.root']}" />
</util:map>
```

```java
@Value("#{fileResourceMap}")
private Map<String, Resource> fileResourceMap;
```

## VM arguments 접근자($)

```bash
${ propertyName }
```

VM arguments(=시스템 프로퍼티)란 jvm을 시작할 때 옵션`-D`으로 추가 가능한 값들을 말한다.

```bash
java -Dproperty1=value1 -Dproperty2=value2 Mainclass
```

#### VM arguments 기본값 설정

```bash
${ propertyName:defaultValue }
```

기본값 설정은 매우 중요한데, 만약 다음의 경우:

```java
@Value("${not-exist-property}")
private String prop;
```

not-exist-property라는 프로퍼티가 없으면 변수 prop의 값은 "${not-exist-property}"로 초기화된다.

그래서 다음처럼:

```java
@Value("${not-exist-property:}")
private String prop;
```

끝에 `:`을 붙여줘서 프로퍼티가 없을 때 empty string으로 초기화되도록 하는게 좋다.

## Types

`T` 연산자를 사용하면 클래스패스 내의 타입을 특정할 수 있다. 이 방법으로 스태틱 메서드 호출도 가능하다고 한다. `java.lang` 패키지의 타입은 패키지를 생략해도 된다.

> You can use the special `T` operator to specify an instance of `java.lang.Class` (the type). Static methods are invoked by using this operator as well. The `StandardEvaluationContext` uses a `TypeLocator` to find types, and the `StandardTypeLocator` (which can be replaced) is built with an understanding of the `java.lang` package. This means that T() references to types within `java.lang` do not need to be fully qualified, but all other type references must be. The following example shows how to use the T operator:
>
>[원문 링크](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#expressions-types)

```java
Class dateClass = parser.parseExpression("T(java.util.Date)").getValue(Class.class);

Class stringClass = parser.parseExpression("T(String)").getValue(Class.class);

boolean trueValue =
    parser.parseExpression("T(java.math.RoundingMode).CEILING < T(java.math.RoundingMode).FLOOR")
            .getValue(Boolean.class);
```
