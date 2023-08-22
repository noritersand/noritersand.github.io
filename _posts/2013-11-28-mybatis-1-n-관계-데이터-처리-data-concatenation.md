---
layout: post
date: 2013-11-28 15:03:00 +0900
title: '[Mybatis] 1:N 관계 데이터 처리 data concatenation'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - concat
  - concatenation
  - resultmap
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.mybatis.org/mybatis-3/](http://www.mybatis.org/mybatis-3/)

## 컬럼명과 필드명의 불일치의 해결은 아주 간단.

보통의 경우, SELECT 결과값은 자바 클래스(자바빈 or Plain Object), HashMap, 단일 값이라면 String 등으로 resultType을 결정한다.

```java
public class User {
    private String userId;
    private String userName;
    private String password;

    // 생략
}
```

resultType으로 자바 클래스를 명시했을 때 만약 두 모델, 즉 자바 클래스와 데이터 모델간 프로퍼티명이 다르면 마이바티스의 자동 매핑이 무력화될 것이다. 따라서 이 경우엔 다음처럼 둘의 이름이 일치하도록 alias를 사용하거나:

```xml
<select id="selectUsers" parameterType="int" resultType="com.someapp.model.User">
    SELECT
        user_id AS userId,
        user_name AS userName,
        hashed_password AS password
    FROM some_table
    WHERE id = #{id}
</select>
```

혹은 resultType을 resultMap으로 대체하는 방법을 택해야 한다:

```xml
<resultMap id="userResultMap" type="com.someapp.model.User">
    <id property="userId" column="user_id" />
    <result property="userName" column="user_name"/>
    <result property="password" column="hashed_password"/>
</resultMap>

<select id="selectUsers" parameterType="int" resultMap="userResultMap">
    SELECT user_id, user_name, hashed_password
    FROM some_table
    WHERE id = #{id}
</select>
```

아니면 카멜 케이스를 스네이크 케이스로 자동 매핑하도록 마이바티스에 설정하는 방법도 있다.

#### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
  </settings>
</configuration>
```

property 속성은 자바 클래스의 프로퍼티(Map 타입의 키값 포함)를, column 속성은 데이터 모델의 컬럼명을 의미한다.

## 1:N 관계의 데이터를 자바 모델로?

하지만 개발을 하다보면 '컬럼명과 필드명의 불일치'같은 간단한 문제만 있는게 아니다. 예를 들어 다음 그림처럼 BBS 테이블과 그것을 참조하는 Attach 테이블이 있다고 가정하자:

![](/images/mybatis-concat-1.png)
(파일첨부가 가능한 게시판의 테이블 관계도)

그리고 특정 글을 보려고 할 때 쿼리가 다음과 같다면:

```xml
<select id="selectBbsDetail" parameterType="java.lang.Integer" resultType="com.model.Bbs">
    SELECT  a.bbs_id, a.name, a.title, a.content,
            b.attach_id, b.file_name, b.file_size
    FROM    bbs a LEFT JOIN attach b
    ON      a.bbs_id = b.bbs_id
    WHERE   a.bbs_id = #{id}
</select>
```

두 테이블의 관계는 1:N 의 관계이므로 하나의 글에 여러 첨부파일이 존재할 수 있다. 때문에 자바코드를 하나의 로우에만 대응하도록 작성했다면 데이터 조회 시점에서 예외가 발생할 가능성이 높다.

쿼리의 결과:

```sql
bbs_id  |  name  |  title  |       content          |  attach_id  |     file_name     |   file_size
---------------------------------------------------------------------------------------------------
  1003      me       제목          내용내용...              24           1e2435.jpg          2349890
  1003      me       제목          내용내용...              25           4e981A.jpg          5799101
  1003      me       제목          내용내용...              25           weeeee.jpg        100382901
```

마이바티스의 에러 메시지:

```java
org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.exceptions.TooManyResultsException
```

위 문제는 DBMS에서 제공하는 **data concatenating**(child rows를 하나의 컬럼으로 출력. MS-SQL이라면 `Cross Apply / FOR XML PATH`, 오라클이라면 LISTAGG를 사용한 방식)으로 해결할 수도 있으나 여기선 resultMap을 활용해보겠다:

```xml
<select id="selectBbsDetail" parameterType="java.lang.Integer" resultType="com.model.Bbs">
    SELECT  a.bbs_id, a.name, a.title, a.content,
            b.attach_id, b.file_name, b.file_size
    FROM    bbs a LEFT JOIN attach b
    ON      a.bbs_id = b.bbs_id
    WHERE   a.bbs_id = #{id}
</select>
```

참조하고 있는 bbsResultMap은 :

```xml
<resultMap id="bbsResultMap" type="java.util.HashMap">
    <id property="bbs_id" column="bbs_id" />
    <result property="name" column="name"/>
    <result property="title" column="title"/>
    <result property="content" column="content"/>
    <collection property="attachList" javaType="java.util.ArrayList" resultMap="bbsAttachResultMap"/>
</resultMap>
```
type을 자바빈이 아닌 HashMap으로 받는것 외엔 차이가 없다. 그러나 위처럼 resultMap으로 매핑할 경우 단순히 컬럼명과 프로퍼티명의 불일치만 해결해주는 것이 아니라 중복되는 결과를 받았을 때 이를 자동으로 걸러내주는 효과도 있다. 이는 하위 요소인 `<id>`를 사용했기 때문이다. `<id>`는 해당 컬럼이 식별자임을 명시하며 전반적인 성능을 향상시킨다고 한다. (더 이상 설명이 없어서...[^1])

그리고 `<collection>`은 **child rows**(1:N의 관계로 설정된 테이블에서 N의 레코드)를 처리하는 방법을 나타낸다. 여기서는 bbsAttachResultMap을 참조하고 있고 이는 다음과 같다:

```xml
<resultMap id="bbsAttachResultMap" type="java.util.HashMap">
    <id property="attach_id" column="attach_id"/>
    <result property="file_name" column="file_name"/>
    <result property="file_size" column="file_size"/>
</resultMap>
```

이제 마이바티스는 다음과 같은 MAP 타입의 값을 돌려 줄 것이다:

```java
{
    bbs_id=1003,
    name=나야,
    title=제목제목,
    content=내용내용..,
    attachList=[
        {
            attach_id=24,
            file_name=1e2435.jpg
            file_size=2349890,
        },
        {
            attach_id=24,
            file_name=4e981A.jpg
            file_size=5799101,
        },
        {
            attach_id=24,
            file_name=weeeee.jpg
            file_size=100382901,
        }
    ]
}
```

[^1]: an ID result; flagging results as ID will help improve overall performance
