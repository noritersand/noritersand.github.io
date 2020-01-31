---
layout: post
date: 2014-11-25 15:44:00 +0900
title: '[Java] org.apache.commons.lang3'
categories:
  - java
tags:
  - java
  - apache.commons.lang3
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://commons.apache.org/proper/commons-lang/javadocs/api-release/index.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/index.html)

아파치 커먼즈(Apache Commons) 프로젝트 중 commons-lang에 속한 API를 분석하여 대~충 정리한 글. API 버전은 commons-lang 3.3.2 기준이다. 3.x로 버전업 되면서 더 이상 하위버전과 호환되지 않으며 이를 명시적으로 표시하기 위해 org.apache.commons.lang3 패키지를 사용한다고 한다.

[http://commons.apache.org/proper/commons-lang/article3_0.html](http://commons.apache.org/proper/commons-lang/article3_0.html)

## ClassUtils

[http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ClassUtils.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ClassUtils.html)

Reflection(sun.java.reflection)을 사용하지 않고 클래스의 메타정보를 생성, 조회한다. 라는 java doc 코멘트가 있지만 소스를 까보면 내부에서 쓰고 있는데 뭘. 아마도 사용자가 직접 Reflect 관련 클래스를 이용할 필요가 없다는 얘기가 아닐까? Class 클래스와 마찬가지로 프레임워크 개발이 아니라면 업무에 사용할 일은 거의 없을 것이다.

#### 주요 메서드

- `convertClassNamesToClasses()`: 클래스명으로 인스턴스 생성
- `getPackageName()`: 특정 클래스가 속한 패키지명 조회
- `isAssignable()`: 서로 다른 타입간 변수값 할당이 가능한지 여부 체크
- `primitiveToWrapper()`: 원시 타입 변수를 wrapper 타입으로 변경

## ObjectUtils

[http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ObjectUtils.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ObjectUtils.html)

전달인자들의 최댓값, 최솟값, 자주 발생하는 값을 산출하는 등의 객체 비교에 사용되는 메서드가 주를 이룬다. 그 외에 toString, equal, hashCode같은 꽤나 사용빈도가 높은 메서드가 있었지만 JDK의 버전이 1.7로 업데이트 되면서 대부분 폐기되었다.

## StringUtils

[http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/StringUtils.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/StringUtils.html)

StringUtils는 문자를 축약하거나 두 문자열을 비교하여 다른 부분을 추출하는 등 String과 관련된 메서드로 구성되어 있고 null safe하다. 몇몇 메서드들은 파라미터의 타입이 String이 아닌 CharSequence라서 StringBuffer나 StringBuilder를 형변환 없이 사용할 수 있다.

- `Empty`: StringUtils의 API에서 empty란 empty string과 null을 의미한다.
- `Blank`: `""`, null, whitespace(공백만으로 이뤄진 문자열)를 의미한다.

## RandomUtils

[http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/RandomUtils.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/RandomUtils.html)

java.util.Random과 비슷하지만 좀 더 사용하기 편하다. 랜덤으로 발생시킬 값들의 최소와 최대를 파라미터로 지정할 수 있기 때문(기존의 Random은 최댓값만, 그것도 integer만 가능하다). 그 외엔 nextBytes 정도(파라미터로 byte[] 객체를 전달하고 되돌려 받는게 아니라 발생시킬 byte 배열의 크기를 지정한다). 정리하자면 기존 API가 범위 지정을 할 수 없는 부분을 개선한 클래스

## ArrayUtils

[http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ArrayUtils.html](http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/ArrayUtils.html)

java.util.Arrays를 대체. 이름에 있다시피 배열 처리를 지원하는 클래스. Arrays와 마찬가지로 원시 타입(primitive type), wrapper 타입(primitive wrapper type) 모두 처리할 수 있다.

#### 주요 메서드

- `add()`: 배열에 요소를 추가한다. 기존 배열을 재구성하는게 아니라 새로운 배열을 생성한다.


TODO
