---
layout: post
date: 2014-01-24 14:48:00 +0900
title: '[JSP] JSTL-fmt'
categories:
  - jsp
tags:
  - java
  - jsp
  - jstl
  - fmt
---

* Kramdown table of contents
{:toc .toc}

```
기능: 지역, 메시지 형식, 숫자 및 날짜형식
directive: <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

국제화 및 지역화 태그로 다국어 문서를 처리할 때 유용하고, 날짜와 숫자 형식을 다룰 때 사용된다. I18N - 국제화(Internationalization) 태그라고도 한다.

## requestEncoding

`request.setCharacterEncoding()`와 같음.

```
<fmt:requestEncoding [value="charsetName"]/>
```

## setLocale

다국어를 지원하는 페이지를 만들때 ResourceBundle로 불러오는 properties 파일들과 연계하여 사용한다.

```
<fmt:setLocale value="locale"
    [variant="variant"]
    [scope="{page|request|session|application}"]/>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>

<%=response.getLocale()%> <!-- ko_KR -->

<fmt:setLocale value="ko"/>
<%=response.getLocale()%> <!-- ko -->

<fmt:setLocale value="ja"/>
<%=response.getLocale()%> <!-- ja -->

<fmt:setLocale value="en"/>
<%=response.getLocale()%> <!-- en -->
```

value의 locale 값은 [언어코드](https://ko.wikipedia.org/wiki/ISO_639)와 [국가코드](https://ko.wikipedia.org/wiki/ISO_3166)로 작성한다. 생략될경우 톰캣 서버의 기본 값으로 설정이 되고, 둘 중에 하나만 사용할 수도 있다.

## bundle

properties 확장자를 사용하는 자원 파일을 읽어오는 역할을 한다.

```
<fmt:bundle basename="basename" [prefix="prefix"]>
  body content
</fmt:bundle>
```

basename 속성에 지정된 properties 파일을 찾아서 locale에 따라 읽어들인다. properties 파일은 보통 `WEB-INF/classes` 아래에 위치하며 디렉터리의 깊이에 따라서 패키지형식의 이름으로 설정한다. TestBundle.properties 파일이 `com/test/msg` 디렉터리에 있다면 `basename="com.test.msg.TestBundle"`  이라고 지정하면 된다. locale이 ko라면 TestBundle_ko.properties 파일을 읽어오게 되며, locale이 맞지 않는 경우에는 TestBundle.properties처럼 코드가 붙지 않은 파일을 읽어온다. prefix 속성은 key 명칭이 공통적인 부분을 지정해서 body에서 표현되는 key를 단축시킨다. import 에서 패키지 명을 지정하면 클래스명만 쓸 수 있는 것과 같이 생각할 수 있다.

## setBundle

페이지 전체에서 사용할 수 있는 번들을 지정한다. var 속성에 명시한 변수를 `<fmt:message/>`태그에서 basename 속성으로 대체할 수 있다.

```
<fmt:setBundle  basename="basename"
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

## message

번들 태그에서 설정한 값을 가져온다.

### Syntax 1: body가 없을때

```
<fmt:message  key="messageKey"
    [bundle="resourceBundle"]
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

### Syntax 2: 메시지 파라미터를 지정하는 body가 있을때

```
<fmt:message  key="messageKey"
    [bundle="resourceBundle"]
    [var="varName"]
    [scope="{page|request|session|application}"]>
  <fmt:param>  서브태그
</fmt:message>
```

### Syntax 3: 키와 선택적 메시지 파라미터를 지정하는 body가 있을때

```
<fmt:message
    [bundle="resourceBundle"]
    [var="varName"]
    [scope="{page|request|session|application}"]>
  key
  선택적 <fmt:param>  서브태그
</fmt:message>
```

bundle 속성으로 번들을 직접 설정할 수도 있고, <fmt:bundle/> 태그 사이에 중첩되어서 키 값만 받아서 출력할 수 있다.

### message tag 적용하기

웹 루트\WEB-INF\classes\bundle 경로에 다음의 properties 파일 두 개를 작성 한다.

#### testBundle_ko.properties

```
name=\ud558\ud558\ud558.
message=JSTL \uc7ac\ubbf8\uc788\ub2e4.
```

#### testBundle.properties

```
name=fixalot
message=JSTL is fun
```

properties 파일을 두 개 작성하는 이유는 로케일에 따라 영어 또는 한글로 출력하기 위함이다. IE는 경우 `인터넷 옵션 > 일반 > 언어`에서 한국어를 지우고 영어를 선택하는 방식으로 테스트 해볼 수 있다.

JSP 파일 작성

```html
<%@ page contentType = "text/html; charset=utf-8"%>
<%@ taglib prefix="c" uri="http://java.sun.comjsp/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>

<html>
<head><title>JSTL fmt - bundle, message</title></head>
<body>
    <fmt:bundle basename="bundle.testBundle">
        <fmt:message key="name"/>
        <p/>
        <fmt:message key="message" var="msg"/>
        <c:out value="${msg}"/>
    </fmt:bundle>
</body>
</html>
```

`<fmt:param>` 적용

```html
<c:set var="userName" value="Guest"/>
<fmt:bundle basename="net.codejava.jstl.messages">
    <fmt:message key="codejava.user.greeting">
        <fmt:param value="${userName}"/>
    </fmt:message>
</fmt:bundle>
```

## formatNumber

숫자의 형식을 표현한다.

### Syntax 1: body가 없을때

```
<fmt:formatNumber value="numericValue"
    [type="{number|currency|percent}"]
    [pattern="customPattern"]
    [currencyCode="currencyCode"]
    [currencySymbol="currencySymbol"]
    [groupingUsed="{true|false}"]
    [maxIntegerDigits="maxIntegerDigits"]
    [minIntegerDigits="minIntegerDigits"]
    [maxFractionDigits="maxFractionDigits"]
    [minFractionDigits="minFractionDigits"]
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

#### Syntax 2: body가 있을때

```
<fmt:formatNumber
    [type="{number|currency|percent}"]
    [pattern="customPattern"]
    [currencyCode="currencyCode"]
    [currencySymbol="currencySymbol"]
    [groupingUsed="{true|false}"]
    [maxIntegerDigits="maxIntegerDigits"]
    [minIntegerDigits="minIntegerDigits"]
    [maxFractionDigits="maxFractionDigits"]
    [minFractionDigits="minFractionDigits"]
    [var="varName"]
    [scope="{page|request|session|application}"]>
  형식화될 수치
</fmt:formatNumber>
```

```html
<fmt:formatNumber value="1700600"/>
<!-- 1,700,600 -->

<fmt:formatNumber value="1700600" type="number"/>
<!-- 1,700,600 -->

<fmt:formatNumber value="1700600" type="number" groupingUsed="false"/>
<!-- 1700600 -->

<fmt:formatNumber value="1700600" type="currency" groupingUsed="true"/>
<!-- ￦1,700,600 -->

<fmt:formatNumber value="1670400" type="currency"  currencySymbol="&"/>
<!-- &1,670,400 -->

<fmt:formatNumber value="0.5" type="percent"/>
<!-- 50% -->

<fmt:formatNumber value="999" minIntegerDigits="5" minFractionDigits="2"/>
<!-- 00,999.00 -->

<fmt:formatNumber value="9876543.61" pattern=".000" />
<!-- 9876543.610 -->

<fmt:formatNumber value="9876543.612345" pattern="#,#00.0#"/>
```

| 속성              | 동적  | Type               | 설명                                                                       |
|-------------------|-------|--------------------|----------------------------------------------------------------------------|
| value             | true  | String 또는 Number |  형식화될 수치                                                             |
| type              | true  | String             |  숫자, 통화, 퍼센트 중 어느 것으로 표시할지 지정 {number\|currency\|percent} |
| pattern           | true  | String             |  사용자가 지정한 형식 패턴.                                                |
| currencyCode      | true  | String             |  ISO 4217 통화 코드 (통화 형식일 때만 적용) type="currency"                |
| currencySymbol    | true  | String             |  통화 기호 (통화 형식일 때만 적용) type="currency"                         |
| groupingUsed      | true  | boolean            |  형식 출력에 세 자릿수 마다 분리기호를 포함할지 여부.  기본값은 true       |
| maxIntegerDigits  | true  | int                |  형식 출력에서 integer 최대 자리 수                                        |
| minIntegerDigits  | true  | int                |  형식 출력에서 integer 최소 자리 수                                        |
| maxFractionDigits | true  | int                |  형식 출력에서 소수점 이하 최대 자리 수                                    |
| minFractionDigits | true  | int                |  형식 출력에서 소수점 이하 최소 자리 수                                    |
| var               | false | String             |  형식 출력 결과 문자열을 담는 scope에 해당하는 변수명                      |
| scope             | false | String             |  var의 scope                                                               |

**IE11은 원화를 표시할 때 소수점 두자리까지 표시하므로 `maxFractionDigits="0"`를 반드시 명시해야한다.**

## parseNumber

formatNumber tag와 반대로 정해진 패턴의 문자열에서 수치를 추출하는 태그

### Syntax 1: body가 없을때

```
<fmt:parseNumber value="numericValue"
    [type="{number|currency|percent}"]
    [pattern="customPattern"]
    [parseLocale="parseLocale"]
    [integerOnly="{true|false}"]
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

### Syntax 2: body가 있을때

```
<fmt:parseNumber
    [type="{number|currency|percent}"]
    [pattern="customPattern"]
    [parseLocale="parseLocale"]
    [integerOnly="{true|false}"]
    [var="varName"]
    [scope="{page|request|session|application}"]>
  파싱할 수치
</fmt:parseNumber>
```

| 속성        | 동적  | Type                         | 설명                                                                        |
|-------------|-------|------------------------------|-----------------------------------------------------------------------------|
| value       | true  | String 또는 Number           |  파싱할 수치                                                                |
|  type       | true  | String                       |  숫자, 통화, 퍼센트 중 어느 것으로 표시할 지 지정 {number\|currency\|percent} |
| pattern     | true  | String                       |  사용자가 지정한 형식 패턴                                                  |
| parseLocale | true  | String 또는 java.util.Locale |  파싱 작업의 기본 형식 패턴(숫자, 통화, 퍼센트 각각)을 제공하는 Locale      |
| integerOnly | true  | boolean                      |  true일 경우 주어진 값에서 Integer 부분만 파싱                              |
| var         | false | String                       |  파싱 결과(java.lang.Number)를 담는 scope에 해당하는 변수명                 |
| scope       | false | String                       |  var의 scope                                                                |

## formatDate

날짜 형식을 표현하는 태그

```
<fmt:formatDate value="date"
    [type="{time|date|both}"]
    [dateStyle="{default|short|medium|long|full}"]
    [timeStyle="{default|short|medium|long|full}"]
    [pattern="customPattern"]
    [timeZone="timeZone"]
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

```html
<jsp:useBean id="now" class="java.util.Date"/>
<c:out value="${now}"/> <!-- Wed Apr 22 17:14:22 KST 2015 -->

<fmt:formatDate value="${now}" type="date"/>
<!-- 2015. 4. 22 -->

<fmt:formatDate value="${now}" type="time"/>
<!-- 오후 5:14:22 -->

<fmt:formatDate value="${now}" type="both"/>
<!-- 2015. 4. 22 오후 5:14:22 -->

<fmt:formatDate value="${now}" type="both" dateStyle="default" timeStyle="default"/>
<!-- 2015. 4. 22 오후 5:14:22-->

<fmt:formatDate value="${now}" type="both" dateStyle="short" timeStyle="short"/>
<!-- 15. 4. 22 오후 5:14 -->

<fmt:formatDate value="${now}" type="both" dateStyle="medium" timeStyle="medium"/>
<!-- 2015. 4. 22 오후 5:14:22 -->

<fmt:formatDate value="${now}" type="both" dateStyle="long" timeStyle="long"/>
<!-- 2015년 4월 22일 (수) 오후 5시 14분 22초 -->

<fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>
<!-- 2015년 4월 22일 수요일 오후 5시 14분 22초 KST -->

<fmt:formatDate value="${now}" type="both" pattern="yyyy년MM월dd일 HH시mm분ss초 E요일"/>
<!-- 2015년04월22일 17시14분22초 금요일 -->
```

| 속성      | 동적  | Type                           | 설명                                                                                                                             |
|-----------|-------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| value     | true  | java.util.Date                 |   형식화 될 Date와 Time                                                                                                          |
| type      | true  | String                         |   형식화 할 데이터가 시간, 날짜, 혹은 시간과 날짜인지 지정                                                                       |
| dateStyle | true  | String                         |   미리 정의된 날짜 형식. java.text.DateFormat 클래스에 정의된 문법을 따른다.   type 속성이 생략되었거나 date 혹은 body일 때 사용 |
| timeStyle | true  | String                         |   미리 정의된 시간 형식.  type 속성이 time 혹은 body일 때 사용                                                                   |
| pattern   | true  | String                         |   사용자 지정 형식 스타일                                                                                                        |
| timeZone  | true  | String 또는 java.util.TimeZone |   형식화 시간에 나타날 타임 존                                                                                                   |
| var       | false | String                         |   형식 출력 결과 문자열을 담는 scope에 해당하는 변수명                                                                           |
| scope     | false | String                         |   var의 scope                                                                                                                    |

## parseDate

정해진 패턴의 문자열에서 날짜를 추출하는 태그

### Syntax 1: body가 없을때

```
<fmt:parseDate value="dateString"
    [type="{time|date|both}"]
    [dateStyle="{default|short|medium|long|full}"]
    [timeStyle="{default|short|medium|long|full}"]
    [pattern="customPattern"]
    [timeZone="timeZone"]
    [parseLocale="parseLocale"]
    [var="varName"]
    [scope="{page|request|session|application}"]/>
```

### Syntax 2: body가 있을때

```
<fmt:parseDate
    [type="{time|date|both}"]
    [dateStyle="{default|short|medium|long|full}"]
    [timeStyle="{default|short|medium|long|full}"]
    [pattern="customPattern"]
    [timeZone="timeZone"]
    [parseLocale="parseLocale"]
    [var="varName"]
    [scope="{page|request|session|application}"]>
  파싱할 수치
</fmt:parseDate>
```

```html
<fmt:parseDate value="20150702030405" pattern="yyyyMMddHHmmss"/>
<!-- Thu Jul 02 03:04:05 KST 2015 -->

<fmt:parseDate pattern="yyyyMMddHHmmss">
20150702030405
</fmt:parseDate>
<!-- Thu Jul 02 03:04:05 KST 2015  -->

<fmt:parseDate value="20150702030405" pattern="yyyyMMddHHmmss" var="sample" scope="page"/>
<fmt:formatDate value="${sample}" pattern="yyyy-MM-dd HH:mm"/>
<!-- 2015-07-02 03:04 -->
<!-- var 옵션을 사용할 경우 화면에 표시하지 않고 프로퍼티에 저장한다. -->
```

| 속성        | 동적  | Type                           | 설명                                                                                                                          |
|-------------|-------|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| value       | true  | String                         |  파싱할 Date String                                                                                                           |
| type        | true  | String                         |  파싱할 데이터가 시간, 날짜, 혹은 시간과 날짜인지 지정                                                                        |
| dateStyle   | true  | String                         |  미리 정의된 날짜 형식. java.text.DateFormat 클래스에 정의된 문법을 따른다. type 속성이 생략되었거나 date 혹은 body일 때 사용 |
| timeStyle   | true  | String                         |  미리 정의된 시간 형식 type 속성이 time 혹은 body일 때 사용                                                                   |
| pattern     | true  | String                         |  사용자 지정 형식 스타일                                                                                                      |
| timeZone    | true  | String 또는 java.util.TimeZone |  형식화 시간에 나타날 타임 존                                                                                                 |
| parseLocale | true  | String 또는 java.util.Locale   |  파싱하는 동안 적용될 미리 정의된 형식스타일의 Locale                                                                         |
| var         | false | String                         |  파싱 결과(java.util.Date)를 담는 scope에 해당하는 변수명                                                                     |
| scope       | false | String                         |  var의 scope                                                                                                                  |

## setTimeZone, timeZone

특정 스코프의 타임 존을 설정하거나 부분 적용한다.

```
<fmt:setTimeZone value="timeZone"
    [var="varName"]
    [scope="{page|request|session|application}"]/>

<fmt:timeZone value="timeZone">
  body content
</fmt:timeZone>
```

```html
<fmt:setLocale value="ko_KR"/>
<jsp:useBean id="now" class="java.util.Date"/>

<c:out value="${now}"/>
<!-- Wed Apr 22 17:29:36 KST 2015 -->

Seoul KST: <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>
<!-- 2015년 4월 22일 수요일 오후 5시 30분 40초 KST -->

<fmt:timeZone value="GMT">
London GMT: <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>
</fmt:timeZone>
<!-- 2015년 4월 22일 수요일 오전 8시 30분 40초 GMT -->

<fmt:timeZone value="GMT-8">
NewYork GMT-8: <fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="full"/>
</fmt:timeZone>
<!-- 2015년 4월 22일 수요일 오전 12시 30분 40초 GMT-08:00 -->
```
