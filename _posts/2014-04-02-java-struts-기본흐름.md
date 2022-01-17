---
layout: post
date: 2014-04-02 18:59:00 +0900
title: '[Java] struts 기본흐름'
categories:
  - java
tags:
  - java
  - struts
  - basics
---

![](/images/struts-1.png)

웹 애플리케이션이 시작할 때 ActionServlet의 init()가 호출되고 web.xml에서 설정한 config 파일(여기선 struts-config.xml)을 로드 한다.

struts-config.xml은 Struts 프레임워크 에서 사용하는 모든 설정 정보를 담고 있는 파일이라고 생각하면 된다. config 이름은 반드시 struts-config.xml이 아니어도 상관없으며 ActionServlet 클래스는 struts에서 제공하고 있다.

![](/images/struts-2.png)

클라이언트가 id와 pw를 입력한 후 submit를 누르면 `action=login.do` 로 이동한다. web.xml에 `*.do` 패턴은 전부 org.apache.struts.action.ActionServlet로 이동하게끔 매핑되어 있다.

![](/images/struts-3.png)

클라이언트가 입력한 id와 pw는 ActionForm 클래스를 상속받는 클래스의 객체에 자동으로 저장된다. 이러한 형식은 struts-config.xml `<form-beans>` 태그에 지정한다.

![](/images/struts-4.png)

클라이언트의 입력 값을 저장한 객체로 어떤 행동을 취할 것인가를 지정한 부분은 struts-config.xml의 `<action-mappings>` 태그다. 이 태그에서 지정한 클래스로 객체를 전달한다. 해당 클래스는 Action 클래스를 상속한 클래스로 DB에 직접 연결하는 클래스를 호출하는 부분이 들어 있다.

![](/images/struts-5.png)

처리한 후 `<action>` 태그에서 지정한 문서로 mapping한다. 전달하는 클래스의 타입은 ActionForward

![](/images/struts-6.png)
