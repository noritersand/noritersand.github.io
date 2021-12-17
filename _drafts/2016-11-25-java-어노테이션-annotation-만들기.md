---
layout: post
date: 2016-11-25 16:14:00 +0900
title: '[Java] 어노테이션(annotation) 만들기'
categories:
  - java
tags:
  - java
  - annotation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- 어디어디

## @Target

```java
@Target({ ElementType.METHOD })
```

메서드 레벨에서만 사용 가능한 어노테이션

```java
@Target({ ElementType.METHOD, ElementType.TYPE })
```

메서드, 타입(클래스) 레벨에서 사용 가능한 어노테이션

## @Retention

**퍼온거니 수정할 것**

Retention은 어노테이션이 얼마나 오랫동안 유지되는지에 대해, JVM이 어떻게 사용자 어노테이션을 다루어야 하는지를 서술합니다.

```java
@Retention(RetentionPolicy.RUNTIME)
```

#### RetentionPolicy

- SOURCE - 어노테이션이 컴파일 타임시 버려진다는 것을 의미합니다. retention정책이 source로 정의되어 있으면, 클래스 파일은 어노테이션을 지니지 못합니다.
- CLASS - 어노테이션이 생성된 클래스 파일에서 나타날 것이라는 것을 의미합니다. 그러나 런타임시에는 이 어노테이션을 이용하지 못합니다.
- RUNTIME- 이는 런타임시 JVM에서 어노테이션의 이용이 가능하다는 것을 의미합니다. 이러한 어노테이션을 읽는 사용자 로직을 가짐으로써 런타임시 무언가를 할 수 있습니다.

## @Inherited

## @Documented


## value(or something)

```java
public @interface DataSourceRoute {
    Connection value();
}
```

요래하면 Connection 타입으로(Enum이 적절)

```java
public @interface DataSourceRoute {
    String value();
}
```

요래하면 String 타입으로 값을 받을 수 있음.

필드의 타입을 배열로 하면:

```java
public @interface DataSourceRoute {
    String[] value();
}
```

배열을 받을 수 있다. 요로케:

```java
@DataSourceRoute({ "one", "two", "three" })
public void getUserList(ExampleParam param) {}
```

그리고 어노테이션의 필드가 딱 하나이고 필드의 이름이 value일 땐 해당 필드가 default 값이 되지만(value란 속성을 명시하지 않아도 된다) 필드의 이름이 value가 아니거나 value 이외의 필드가 또 있을 땐(즉, 필드가 둘 이상일 때) default 키워드를 사용해야 속성명을 생략할 수 있게 된다. **확인할 것**

## @AliasFor

```java
public @interface DataSourceRoute {
    @AliasFor("value")
    Connection[] conn();
}
```

요래하면

```java
@DataSourceRoute(conn = Connection.READ_ONLY)
public void getUserList(ExampleParam param) {}
```

요래 해야 하고

```java
public @interface DataSourceRoute {
    Connection[] value();
}
```

요래 하면

```java
@DataSourceRoute(Connection.READ_ONLY)
public void getUserList(ExampleParam param) {}
```

요렇게만 됨. 안 지키면 컴파일 에러

근데 요래 하면

```java
public @interface DataSourceRoute {
    @AliasFor("value")
    Connection[] conn() default {};

    @AliasFor("conn")
    Connection[] value() default {};
}
```

conn이 value고 value가 conn이므로 둘 다 받을 수 있음. 짱짱맨ㅋ

## default

생략해도 되는 값은 default 키워드를 사용해 기본값을 할당해준다.

```java
public boolean clear() default false;
```

위의 경우 명시하지 않으면 false로 설정된다.

스프링의 [RequestMapping](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html) 타입 참고.

```java
package org.springframework.web.bind.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.util.concurrent.Callable;

import org.springframework.core.annotation.AliasFor;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
    String name() default "";

    @AliasFor("path")
    String[] value() default {};

    @AliasFor("value")
    String[] path() default {};

    RequestMethod[] method() default {};

    String[] params() default {};

    String[] headers() default {};

    String[] consumes() default {};

    String[] produces() default {};
}
```
