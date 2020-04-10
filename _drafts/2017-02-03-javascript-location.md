---
layout: post
date: 2017-02-03 13:38:00 +0900
title: '[JavaScript] Location'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - location
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/API/Location](https://developer.mozilla.org/ko/docs/Web/API/Location)
- [https://developer.mozilla.org/en-US/docs/Web/API/Window/location](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)

- location.toString
- location.href
- location.path
- location.search
- location.hash

.href 와 .repalce()는 모두 location의 하위객채로 브라우저에서 URL이동때 쓰인다.

그러나 쓰는 형태를 보면 알겠지만 .href 는 프로퍼티고, .replace()는 메서드다.

location.href = http://www.naver.com        <= [1] 값을 정의해야 하는프로퍼티

location.replace(http://www.naver.com)    <= [2] 파라미터로 작동을 명령하는 메서드

아. 그게 뭐가 중요하냐... 브라우저가 주소만 바뀌면 되는거 아냐... 라고 하겠지만..

그게 아니라 이거지... ㅡ ㅡa

골아프겠지만, 자바스크립트에서 정의한 정확한 의미를 집어보자.

location 은 현재 브라우저에 떠있는 URL 주소값에 관련된 내용을 다루는 객체다.

브라우저의 주소표시줄에 있는 URL은 다음과 같이 정의된다.

protocol :// hostname : port / pathname ? search # hash

location.href는 위에 써있는 전체를 가르키며,

location.pathname 이라고 하면 같은 사이트에 파일경로만을 가르킨다.

(예를 들면...http://www.naver.com/blog/myinfo/profile.asp  라는 페이지가 떠있따면...

location.href에는 이거 전체가, location.phthname 에는 [blog/myinfo/profile.asp] 가 들어있다.)

그래서 location.href 라고 하면 브라우저의 주소표시줄에 떠있는 URL를 가르킨다.

그러므로 [1]처럼하면 브라우저의 주소표시줄 값이 변경되므로 페이지가 바뀌게 된다.

(물론 프레임, 아이프레임을 썼을땐 그 프레임의 주소가 바뀐다.)

[1]을 했을때 일어나는 일은 우리가 주소표시줄에 키보드로 직접 주소를 넣고 엔터를 치는것과 정확히 같은 일을 일으킨다.

여기서 같은 일이란,

새로은 페이지로 이동(a)되고,

[뒤로]버튼을 누르면 이전 URL로 이동(b)되는것을 말한다.

(a) , (b)에 대해 좀만더 자세히 보자.

(a)  새로운 페이지로 이동.

브라우저 옵션을 손대지 않았을때, 브라우저의 주소값이 바뀌면 브라우저는 '인터넷 임시파일'

(C:\Documents and Settings\Administrator\Local Settings\Temporary Internet Files\)

에 캐쉬가 있는지를 먼저 보고, 있으면 그걸 보여준다.

그래서 가끔 우린 사이트내용이 바뀌었는데도, 로컬에 있는 파일을 보는 경우가 있다.

location.href로 주소이동을 했을때 이와 같은 일이 일어난다.

(b) [뒤로]버튼을 누르면 이전 URL로 이동.

[뒤로]버튼이 정상장동되는것은 History객체에 배열처럼 이전 URL들이 기록되어있기때문이다.

우리가 [뒤로]버튼을 누르는건 History객체를 역순으로 되집어 가는 과정이다. ( history.back()이 그 일을 한다. )

location.href를 쓰면 [뒤로]버튼도 history.back()도 직접URL바꿨을때와 똑같이 작동한다.



그럼 location.replace()는 뭐가 다를까?

location.repalce()는 다음과 같이 작동한다.



1. location.replace()는 (a)의 경우  '인터넷 임시파일'을 쓰지 않는다. 매소드가 실행될때마다 매번 서버에 접속해서 페이지를 가져온다. 게시판 리스트같은 곳을 이동할때 location.href를 쓰면 새 글이 올라온것을 모르고 '로컬에 있는 파일'만 보는 일이 생길 수 있는데, location.replace()를 쓰면 이를 방지할 수 있다.



2. location.replace()은 새 페이지로 이동하는게 아니라 현재페이지를 바꿔주는 거다.

말장난 같아도 이거 중요한거다.. 왜중요한고하니...

(b)의 경우, History객체에 새로운 URL를 기록하는게 아니라 현재 페이지값을 바꾼다.

그러므로 location.replace()로 이동하고 [뒤로]버튼을 누르면 이전페이지가 아니라 이전,이전페이지가 뜬다. 이해가 안된다고?



A --> B --> C    처럼 페이지가 이동을 했다하자. (현재 당신은 C사이트에...)

B --> C로 이동할때 location.href를 썼다면

C페이지트에서 [뒤로]버튼을 누르면 B가뜬다.

하지만..

B --> C로 이동할때 location.replace()를 썼다면

C페이지에서 [뒤로]버튼을 누르면 A가뜬다.

그럼 사용자입장에선 '어 내가 클릭을 두번했나?' 하게 된다...



이런 차이로 인하여 적절히 써야 한다.

[뒤로]버튼을 눌렀을때 두페이지 이전으로 넘어가면 안되는 경우가 있는 반면,(.href를 써야겠지..)

프레임을 쓴 사이트 의 경우는 [뒤로]버튼 한두번 클릭으로 사이트를 빠져나가게 할 수도 있다. (.repalce()를 쓴경우...)
