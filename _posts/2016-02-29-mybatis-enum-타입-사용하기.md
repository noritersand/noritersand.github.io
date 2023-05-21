---
layout: post
date: 2016-02-29 13:54:00 +0900
title: '[Mybatis] Enum 타입 사용하기'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - enum
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://groups.google.com/forum/#!topic/mybatis-user/uKz_aOYMz4o](https://groups.google.com/forum/#!topic/mybatis-user/uKz_aOYMz4o)

#### 테스트 환경

- mybatis 3.3.1
- mybatis-spring 1.2.4


## 분기 조건으로 사용할 때

enum 타입의 파라미터를 if문의 변수로 사용한다고 하자. 가령 검색 조건으로 사용될 클래스가 다음처럼 enum 타입을 포함하고 있을 때:

```java
enum SystemDivision {
    BACK_OFFICE, FRONT_OFFICE
}

public class SearchCondition {
    private SystemDivision sysDiv;

    // getter, setter
}
```

mapper에서 `Enum.name()` 혹은 `Enum.toString()`을 직접 호출하도록 작성하면, enum 타입의 값을 문자열과 비교하는 조건으로 사용할 수 있다:

```xml
<select id="getSomething" parameterType="SearchCondition">
    SELECT 'something' FROM my_test_table
    <if test="sysDiv.name() == 'FRONT_OFFICE'" >
        WHERE 1 = 1
    </if>
    <if test="sysDiv.toString() == 'BACK_OFFICE'">
        WHERE 2 = 2
    </if>
</select>
```

`Enum.name()`이나 `Enum.toString()` 모두 enum 타입의 이름을 돌려주니 두 메서드의 결과는 같다. 아래는 실제 Enum타입의 소스:

```java
public abstract class Enum<E extends Enum<E>>
        implements Comparable<E>, Serializable {

    private final String name;

    public final String name() {
        return name;
    }

    public String toString() {
        return name;
    }

    // 생략
}
```


## 파라미터 표현식에서 사용할 때

마이바티스는 파라미터 표현식에 사용된 문자열을 기준으로 getter를 찾으려하기 때문에 다음처럼 임의의 getter를 만드는 방식을 적용한다.

```java
enum SystemDivision {
    BACK_OFFICE, FRONT_OFFICE

    public String getValue() {
        return super.toString();
    }
}

public class SearchCondition {
    private SystemDivision sysDiv;

    // getter, setter
}
```

```xml
<if test="sysDiv != null">
    #{sysDiv.value}
</if>
```
