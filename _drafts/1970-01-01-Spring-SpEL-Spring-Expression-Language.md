---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Spring: SpEL, Spring Expression Language'
categories:
  - java
  - spring
tags:
  - todo
  - spring
  - SpEL
---

#### 참고한 글
- http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html
- https://blog.outsider.ne.kr/835

## 스프링 빈 접근자(#)
```bash
#{ beanId }
```

#### 스프링 빈의 프로퍼티에 접근
```bash
#{ beanId.propertyName }
#{ beanId[ 'propertyName' ] }
```

#### example#1
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

#### example#2
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

## 자바 프로퍼티 접근자($)
```bash
${ propertyName }
```

자바 프로퍼티(혹은 JVM 프로퍼티)란 jvm을 시작할 때 옵션`-D`으로 추가가능한 값들을 말한다.
```bash
java -Dproperty1=value1 -Dproperty2=value2 Mainclass
```

#### 자바 프로퍼티 기본값 설정
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
끝에 `:`을 붙여줘서 프로퍼티가 없을 때 nullstring으로 초기화되도록 하는게 좋다.

## TODO
`#{systemProperties['PROPERTY_NAME']}`, `#{systemEnvironment['ENV_VARIABLE_NAME']}` 둘의 차이 확인할 것.
