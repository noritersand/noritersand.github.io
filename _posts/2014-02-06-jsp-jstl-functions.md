---
layout: post
date: 2014-02-06 17:47:00 +0900
title: '[JSP] JSTL-Functions'
categories:
  - jsp
tags:
  - java
  - jsp
  - jstl
  - functions
---

* Kramdown table of contents
{:toc .toc}

```
기능: 문자열, 맵/컬렉션 처리
directive: <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

JSTL functions은 문자열이나 맵/컬렉션의 가공/변환/분석을 위해 사용하는 함수다. 단독으로는 쓸 수 없고 반드시 EL 표현식과 조합해야 한다.

## contains, containsIgnoreCase

```
boolean contains( String sting, String substring )
```

string이 substring을 포함하면 true값을  반환한다.

```
boolean containsIgnoreCase( String string, String substring )
```

대소문자에 관계없이, string이 substring을 포함하면 true 반환.

```java
${fn:contains("helloworld", "world")} // true
${fn:containsIgnoreCase("hello world!", "WoRLd")} // true
```

## startsWith, endsWith

```
boolean startsWith( String string, String prefix )
```

string이 prefix로 시작하면 true

```
boolean endsWith( String string, String suffix )
```

string이 suffix로 끝나면 true

```java
${fn:startsWith("hello world!", "ld!")} // false
${fn:endsWith("hello world!", "ld!")} // true
```

## escapeXml

```
String escapeXml( String string )
```

string에서 XML, HTML의 `<`, `>`, `&`, `'`, `"` 문자들을 각각 `&lt;`, `&gt;`, `&amp;`, `&#039;`, `&#034;`[^1]로 변환한다.

```java
<c:out value="${fn:escapeXml('<>')}"/> // &lt;&gt;
```

## indexOf

```
int indexOf( String string, String substring )
```

string에서 substring의 첫 위치에 해당하는 인덱스를 반환한다.

```java
${fn:indexOf("abcdefg", "f")} // 5
```

## split, join

```
String[] split( String string, String separator )
```

string 내의 문자열을 separator를 기준으로 잘려진 문자열들을 배열 형태로 반환한다.

```
String join( String[], String separator )
```

배열 원소들을 모두 연결해 반환한다. 이때 각 원소 사이에 separator를 구분자로 사용한다.

```java
<c:set var="texts" value="${fn:split('Hi My name is waldo', ' ')}"/>
<c:out value="${fn:join(texts, '-')}"/> // Hi-My-name-is-waldo
```

참고로 EL에선 배열 리터럴[^2]만으로는 배열 생성-초기화가 불가능하다. `fn:split`으로 기존 문자열을 쪼개거나 액션태그인 `jsp:useBean`을 사용해야 한다.

## length

```
int length( Object  item )
```

item이 배열이나 컬렉션이면 원소의 개수를, 문자열이면 문자의 길이를 반환한다.

```java
<c:set var="texts" value="${fn:split('Hi My name is waldo', ' ')}"/>
${fn:length(texts)} // 5
${fn:length("123456")} // 6
```

## replace

```
String replace( String string, String before, String after )
```

string 내에 있는 before를 after로 모두 바꿔서 반환한다.

```java
${fn:replace("hi hello", "hello", "hi")} // hi hi

// replace 함수는 HTML에서 공백과 줄 바꿈을 표현할 때 사용할 수 있다.
${fn:replace("hell            o          o       ~", " ", " ")} // hell            o          o       ~

<% pageContext.setAttribute("enter","\n"); %>
${fn:replace(info.text, enter, '<br/>') // 엔터처리

<% pageContext.setAttribute("enter","\n"); %>
${fn:replace(fn:replace(fn:escapeXml(info.content_txt), enter, '<br/>') , ' ', ' ')} // 엔터와 공백 처리
```

## substring, substringAfter, substringBefore

```
String substring( String string, int begin, int end )
```

string에서 인덱스가 begin과 end 사이에 해당하는 문자열을 반환.

```
String substringAfter( String string, String substring )
```

string에서 substring이 나타나는 이후의 부분에 있는 문자열을 반환한다.

```
String substringBefore( String string, String substring )
```

string에서 substring이 나타나기 이전의 부분에 있는 문자열을 반환한다.

```java
${fn:substring(text, 3, 19)} // My name is waldo
${fn:substringAfter(text, "Hi ")} // My name is waldo
${fn:substringBefore(text, "waldo")} // Hi My name is
```

## toLowerCase, toUpperCase

```
String toLowerCase( String string )
```

string을 모두 소문자로 변환.

```
String toUpperCase( String string )
```

string을 모두 대문자로 변환.

```java
<c:set var="text" value="Hi My name is waldo"/>
${fn:toLowerCase(text)} // hi my name is waldo
${fn:toUpperCase(text)} // HI MY NAME IS WALDO
```

## trim

```
String trim( String string )
```

string 앞뒤의 공백을 제거한다.

```java
<c:set var="text" value="          blank spcae          "/>
${fn:length(text)}  // 31
<c:set var="text" value="${fn:trim(text)}"/>
${fn:length(text)}  // 11
```

[^1]: xml character entities
[^2]: { 1, 2, 3, ... }
