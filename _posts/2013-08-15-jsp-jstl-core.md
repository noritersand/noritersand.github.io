---
layout: post
date: 2013-08-15 00:00:00 +0900
title: 'JSP: JSTL-Core'
categories:
  - jsp
tags:
  - java
  - jsp
  - jstl
  - core
---

* Kramdown table of contents
{:toc .toc}

```
기능: 변수지원, 흐름제어, URL처리
directive: <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

## 기능별 구분

- EL 지원 기능: catch, out, remove, set
- 흐름 제어 기능: choose, forEach, forTokens, if
- URL 관리기능: import, param, redirect, param, url

## catch

body 위치에서 실행되는 코드의 예외를 잡아내는 역할을 담당한다.

```html
<c:catch var="name">
  body content
</c:catch>
```

```html
<%@ page contentType = "text/html; charset=utf-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<h3>코어</h3>
<h4><c:out></h4>
<pre>
${1+2} <c:out value="${1+2}"/>
${1>3} <c:out value="${1>3}"/>
${1 gt 3} <c:out value="${1 gt 3}"/>

\$표시 <c:out value="${'${'}test}"/>

escapeXml 속성 기본 값은  false
false: <c:out value="<b>bold</b> <,>,&,',\" " escapeXml="false"/>
true: <c:out value="<b>bold</b> <,>,&,',\" " escapeXml="true"/>

" 큰따옴표 사용주의  ' 작은따옴표로 대치
<c:out value='<font color="blue">파랑</font>'/>
<c:out value='<font color="blue">파랑</font>' escapeXml="false"/>

<hr><h4><c:set></h4>
set session scope var "name": <c:set var="name" value="하늘" scope="session"/>
c:out name: <c:out value="${name}"/>
expression name: <%=session.getAttribute("name")%>
set page scope var "name": <c:set var="name">
   hello
</c:set>
c:out name: <c:out value="${pageScope.name}"/>
c:out sessionScope.name: <c:out value="${sessionScope.name}"/>
expression name: <%=session.getAttribute("name")%>

<hr><h4><c:remove></h4>
remove session scope var "name": <c:remove var="name" scope="session"/>
expression name: <%=session.getAttribute("name")%>
c:out sessionScope.name: <c:out value="${sessionScope.name}"/>

<hr><h4><c:catch></h4>
<c:catch var="errmsg">
line1
<%=1/0%>
line2
</c:catch>
<c:out value="${errmsg}"/>
</pre>
```

## choose-when-otherwise

`<c:choose/>` 태그는 java의 switch 문과 같지만, 조건에 문자열 비교도 가능하고 쓰임의 범위가 넓다. 또한 `<c:if/>` 태그에 else가 없기 때문에 이의 대체 기능도 수행한다.

```html
<c:choose>
  body content
  (하나 이상의 <when>과 하나 이하의 <otherwise> 서브태그)
  <c:when test="조건_1">
    body content
  </c:when>
  <c:when test="조건_2">
    body content
  </c:when>
  <c:otherwise>
    conditional block
  </c:otherwise>
</c:choose>
```

```html
<%@ page contentType = "text/html; charset=utf-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core"  prefix="c"%>

<h3>조건</h3>
파라미터 없음:<c:out value="${empty param.name}"  />
<h4><c:if test=""></h4>
<c:if test="${empty param.name}">
<form>
이름을 적어주세요.<br/>
     <input type="text" name="name"/> <input type="submit" value="확인"/>
</form>
</c:if>
<c:if  test="${!empty param.name}">
    안녕하세요. <c:out value="${param.name}"/>님.
</c:if>

<h4><c:choose> <c:when test=""> <c:otherwise></h4>
<c:choose>
     <c:when test="${empty param.name}">
         <form>
        이름을 적어주세요.<br/>
             <input type="text" name="name"/> <input type="submit" value="확인"/>
         </form>
     </c:when>
     <c:when test="${param.name=='admin'}">
        안녕하세요. 관리자님.
     </c:when>
     <c:otherwise>
        안녕하세요. <c:out value="${param.name}"/>님.
     </c:otherwise>
</c:choose>
```

```html
<c:choose>
    <c:when test="${param.num1%12==0}">
        ${param.num1 }은 3과 4의 배수<br/>
    </c:when>
    <c:when test="${param.num1%3==0}">
        ${param.num1 }은 3의 배수<br/>
    </c:when>
    <c:when test="${param.num1%4==0}">
        ${param.num1 }은 4의 배수<br/>
    </c:when>
    <c:otherwise>
        ${param.num1 }은 3또는 4의 배수가 아님<br/>
    </c:otherwise>
</c:choose>
```

## forEach

반복실행 태그.

### Syntax 1: 객체 전체에 걸쳐서 반복

```html
<c:forEach [var="varName"] items="collection" [varStatus="varStatusName"]
  [begin="begin"] [end="end"] [step="step"]>
  body content
</c:forEach>
```

### Syntax 2: 지정한 횟수만큼 반복

```html
<c:forEach [var="varName"] [varStatus="varStatusName"]
  begin="begin" end="end" [step="step"]>
  body content
</c:forEach>
```

`<c:forEach/>` 태그는 여러 가지로 활용이 가능하다. 원하는 구간만큼 반복할 수도 있고, 객체를 받아와서 그 객체의 길이만큼 반복할 수도 있다. begin, end 속성은 시작번호와 끝 번호를 지정하고, step 속성을 이용해서 증가 구간을 정할 수 있다. var 속성에서 정한 변수로 반복되는 내부 구간에서 사용할 수 있다.

```html
<table>
     <tr><td>Value</td>
       <td>Square</td></tr>
   <c:forEach  var="x"  begin="0"  end="10"  step="2">
     <tr><td><c:out  value="${x}"/></td>
         <td><c:out  value="${x  *  x}"/></td></tr>
   </c:forEach>
</table>
```

컬렉션의 멤버들 사이를 반복할 때 `<c:forEach>` 태그의 추가 애트리뷰트인 items 애트리뷰트가 사용된다.

```html
<c:forEach var="dto" items="${list}">
    이름:${dto.name}, 제목:${dto.subject}<br/>
</c:forEach>
```

varStatus은 상태 값을 의미하며 다음의 값을 갖는다.

- current: 현재 아이템
- index: 0부터의 순서
- count: 1부터의 순서
- first: 현재 루프가 처음인지 반환
- last: 현재 루프가 마지막인지 반환
- begin: 시작값
- end: 끝값
- step: 증가값

```html
<c:forEach items="${RESULT}" var="RESULT" varStatus="status">
    forEach의 현재 아이템은 : ${status.current}
</c:forEach>
```
```html
<c:forEach var="n" begin="1" end="9">
    ${param.num}*${n}=${param.num*n}<br/>
</c:forEach>

<c:forEach var="dto" items="${list}">
    이름:${dto.name},
    제목:${dto.subject}<br/>
</c:forEach>

<c:forEach var="dto" items="${list}" varStatus="s">
    이름:${dto.name},
    제목:${dto.subject}<br/>
    현재아이템:${s.current}<br/>
    0부터의순서:${s.index}<br/>
    1부터의순서:${s.count}<br/>
    처음이냐?:${s.first}<br/>
    마지막이냐?:${s.last}<br/>
</c:forEach>

<c:forEach var="dto" items="${lists}">
    <dl>
        <dd class="num">${dto.num}</dd>
        <dd class="subject"><a href="">${dto.subject}</a></dd>
        <dd class="name">${dto.name}</dd>
        <dd class="created">${dto.created}</dd>
        <dd class="hitCount">${dto.hitCount}</dd>
    </dl>
</c:forEach>
```

## forTokens

`<c:forTokens/>`는 java.util.StringTokenizer의 기능을 태그로 구현한 것이다.

```html
<c:forTokens items="stringOfTokens" delims="delimiters"
  [var="varName"] [varStatus="varStatusName"]  [begin="begin"]  [end="end"]  [step="step"]>
  body  content
</c:forEach>
```

```html
<%@ page contentType = "text/html; charset=utf-8"%>
<%@ taglib  uri="http://java.sun.com/jsp/jstl/core"  prefix="c"%>

<h3>반복</h3>
<h4><c:forEach></h4>

<c:forEach var="one" begin="1" end="10">
     <c:out value="${one}"/>
</c:forEach>
<p><b>header</b></p>
<c:forEach var="h" items="${header}">
     <c:out value="${h.key}:${h.value}"/><br/>
</c:forEach>

<h4><c:forTokens></h4>
<c:forTokens var="one" items="서울|인천,대전,대구,부산,광주,평양"
             delims="," varStatus="sts">
     <c:out value="${sts.count}:${one}"/>,
</c:forTokens>
<hr/>

<c:forTokens var="one"
             items="서울|인천,대전,대구,부산,광주,평양"
             delims=",|" varStatus="sts">
     <c:out value="${sts.count}:${one}"/>,
</c:forTokens>
```

## import

`<c:import/>` 는 웹 어플리케이션 내부의 자원이나, http, ftp 같은 외부에 있는 자원을 가져와서 페이지 내에 귀속시킨다. 자유롭게 가공할 수도 있고, 편집도 가능하다.

### Syntax 1: 해당 주소를 바로 출력하거나 String 에 담아놓는다.

```html
<c:import url="url" [context="context"]
    [var="varName"]  [scope="{page|request|session|application}"]
    [charEncoding="charEncoding"]>
  <c:param> 서브 태그 위치
</c:import>
```

### Syntax 2: 해당 주소의 컨텐츠를  Reader 객체로

```html
<c:import url="url" [context="context"]
    varReader="varReaderName"
    [charEncoding="charEncoding"]>
  varReader 를 사용하는 액션
</c:import>
```

```html
<%@ page contentType = "text/html; charset=utf-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<c:set var="url" value="http://www.daum.net/"/>
<c:import url="${url}" var="u"/>
<c:out value="${url}"/> 가져옵니다.
<hr/>
-- 홈페이지 결과 출력<br/>
  <c:out value="${u}" escapeXml="false"/>

<hr/>
<c:set var="url" value="http://www.naver.com"/>
<c:import url="${url}" var="u"/>
<c:out value="${url}"/> 가져옵니다.
<hr/>
-- 소스 출력<br/>
<pre><c:out value="${u}"/></pre>

<hr/>
<c:set  var="url" value="gugu.jsp"/>
<c:import url="${url}" var="u">
    <c:param  name="su"  value="5"/>
</c:import>
<c:out value="${url}"/> 가져옵니다.
<hr/>
<c:out value="${u}"  escapeXml="false"/>
<hr/>
```

## if

조건문으로 사용한다.

### Syntax 1: Body 없는 경우

```html
<c:if test="testCondition" var="varName" [scope="{page|request|session|application}"]/>
```

### Syntax 2: Body 있는 경우

```html
<c:if test="testCondition" [var="varName"] [scope="{page|request|session|application}"]>
  body content
</c:if>
```

```html
<form method="post">
수: <input type="text" name="num">
<input type="submit" value="결과">
</form>
//액션이 없어서 자기 자신에게 parameter를 전송하는 폼

<c:if test="${not empty param.num }">
    <c:if test="${param.num % 2 == 0}">
        ${param.num}은 짝수<br/>
    </c:if>
    <c:if test="${param.num % 2 != 0 }">
        ${param.num}은 홀수<br/>
    </c:if>
</c:if>


<c:if test="${dataCount==0}">
    등록된게시물이음슴
</c:if>

<c:if test="${dataCount!=0}">
    ${pageIndexList}
</c:if>
```

`<c:if/>` 에서 나온 결과를 varName 변수에 넣고, 나중에 활용이 가능하다. 변수의 scope는 임의로 지정할 수 있고, 생략될 경우 기본 값은 page 이며 else 문은 존재 하지 않는다.

### 자바스크립트 제어

```html
<c:if test="${sessionScope.session.userId == dto.userId}">
  function updateBoard(boardNum) {
    var url = "/board/update.action";
    location.href = url;
  }
</c:if>
```

이렇게들 많이 쓰지만 사실 자바스크립트 쪽은 JSTL 없이 별도의 js 파일로 분리하는게 좋다. (캐시와 난독화 적용하려면...)

## out

JSP expression[^1]을 대체하는 태그로 가장 많이 사용된다... 는 옛말이고 지금은 XML을 escape 처리할 때나 사용되는 태그.

### Syntax 1: body 없는 경우

```html
<c:out value="value" [escapeXml="{true|false}"] [default="value가 없을 때 표시될 문자"]  />
```

### Syntax 2: body 있는 경우

```html
<c:out value="value" [escapeXml="{true|false}"]  />
  value가 없을 때 표시될 문자
</c:out>
```

```html
Hello <c:out value="${user.username}" default="Guest"/>!

<c:out value="${user.company}" escapeXml="false"/>

<c:set var="timezone" scope="session">
   <c:out value="${cookie['tzPref'].value}" default="CST"/>
</c:set>

<c:out value="&lt; &gt;" escapeXml="true"/> <!-- &amp;lt;&amp;gt; -->
<c:out value="&lt; &gt;" escapeXml="false"/> <!-- &lt;&gt; -->
```

escapeXml 속성은 XML Entities의 치환여부를 의미한다. 생략 혹은 true로 설정할 경우 value에 XML entities로 치환가능한 특수문자가 있을 경우 치환하여 표시한다. 반대로 false는 value를 있는 그대로 표시한다.

다시 정리하면, `&#58;`라는 값을 출력할 때 `escapeXml="false"`일 땐 `&#58;` 문자 그대로 응답하고 브라우저가 이를 `:`으로 치환한다. 반면 `escapeXml="true"`면 `&#58;`를 그대로 출력하기 전에 한 번 더 escape 해서 `&amp;#58;`라는 문자를 응답한다. 브라우저가 보기에 `&amp;#58;`에서 치환해야하는 문자는 `&amp;` 뿐이므로 최종적으로는 `&;#58;`가 페이지에 표시되는 것이다. ~~헥헥~~

## param

보통 import, redirect, url 태그와 같이 쓰이는 태그. "파라미터=값" 형식을 표현한다.

```html
<c:param 속성="값".../>
```

```html
<c:import url="/exec/doIt">
   <c:param name="action" value="register"/>
</c:import>

<!-- 이 방법은 아래 태그와 같은 효과가 있다 -->
<c:import  url="/exec/doIt?action=register"/>
```

## redirect

`response.sendRedirect()`를 대체하는 태그. 컨텍스트를 지정해서 다른 컨텍스트로 이동이 가능하다.

### Syntax 1: Body 없는 경우

```html
<c:redirect url="value" [context="context"]/>
```

### Syntax 2: Body 있는 경우 쿼리 스트링 파라미터 지정

```html
<c:redirect url="value" [context="context"]/>
  <c:param> 서브태그
</c:redirect>
```

```html
<% response.setContentType("text/html");%>
<%@ taglib uri="http://java.sun.com/jstl/core" prefix="c"%>

<c:redirect url="/test.jsp"/>
```

## set, remove

set은 `HttpServletRequest.setAttribute()`와 역할이 같다. scope 속성으로 page, request, session, application 중 하나의 유효범위를 설정할 수 있다.

remove는 JSP의 `removeAttribute()`와 같은 역할을 한다.  (page, request, session, application) 범위의 변수(속성)를 제거한다.

### Syntax 1: scope 에 해당하는 변수에 속성 값을 정한다.

```html
<c:set value="value" var="varName" [scope="{page|request|session|application}"]/>
```

### Syntax 2: scope 에 해당하는 변수에  body 값을 정한다.

```html
<c:set var="varName" [scope="{page|request|session|application}"]>
  body content
</c:set>
```

### Syntax 3: 속성 값으로  target 객체의 프로퍼티 값을 정한다.

```html
<c:set value="value" target="target" property="propertyName"/>
```

### Syntax 4: body 값으로  target 객체의 프로퍼티 값을 정한다.

```html
<c:set target="target" property="propertyName">
  body content
</c:set>
```

scope 속성이 생략될 경우 기본 값은 page다. 그리고 var 속성과 target 속성은 동시에 사용할 수 없다.

```html
<c:set var="testObject" value="hello world!"/> // request.setAttribute("testObject", "hello world!")
<c:out value="${testObject}"/> // request.getAttribute("testObject")
```

var속성의 경우는 비교적 간단하다:

```html
<jsp:useBean id="map" class="java.util.HashMap"/>
<c:set target="${map}" property="hi" value="hello world!"/> // map.put("hi", "hello world!")
<c:out value="${map.hi}"/> // map.get("hi")

<jsp:useBean id="vo" class="com.test.TestVO"/>
<c:set target="${vo}" property="name" value="who?"/> // vo.setName("who?")
<c:out value="${vo.name}"/> // vo.getName()

<c:forEach items="${sampleList}" var="list">
<c:set target="${list}" property="col1" value="1234"/> // sampleList.get(loop).setCol1("1234")
    <tr>
        <td>${list.col1}</td> // sampleList.get(loop).getCol1()
        <td>${list.col2}</td>
        <td>${list.col3}</td>
        <td>${list.col4}</td>
        <td>${list.col5}</td>
        <td>${list.col6}</td>
    </tr>
</c:forEach>
```

target속성은 jsp에서 객체 생성 후 사용하거나 기존 객체의 getter/setter를 이용하는 방식이다.

## url

`<c:url/>` 태그는 컨텍스트를 자동으로 추가해서 주소를 자동으로 생성해준다.  context 속성이 지정되었을 경우 value와 context 의 값은 `/`로 시작을 해야  된다. context 속성이 생략되면 당연히 현재의 컨텍스트가 적용된다.

### Syntax 1: Body 없는 경우

```html
<c:url value="value" [context="context"] [var="varName"] [scope="{page|request|session|application}"]/>
```

### Syntax 2: Body 있는 경우 파라미터 지정

```html
<c:url value="value" [context="context"] [var="varName"] [scope="{page|request|session|application}"]>
  <c:param> 서브태그
</c:url>
```

```html
<% response.setContentType("text/html");%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<c:url value="/images/tomcat.gif"/>
<img src='<c:url value="/images/tomcat.gif"/>
```

[^1]: <%= %> [본문으로]
