---
layout: post
date: 2017-05-04 11:18:00 +0900
title: 'Spring: @SessionAttribute와 SessionStatus'
categories:
  - spring
tags:
  - java
  - spring
  - sessionattribute
  - sessionstatus
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://java.ihoney.pe.kr/218](http://java.ihoney.pe.kr/218)
- [https://offbyone.tistory.com/333](https://offbyone.tistory.com/333)

여기 참고혀. 참고만하고 언제쓰냐.

## @SessionAttributes

## SessionStatus

## @ModelAttribute와 조합

요딴식 `@SessionAttributes`의 키를 설정하고:

```java
@SessionAttributes({"loginId"})
@Controller
public class TestController {

}
```

요로코롬 model에 attribute를 설정하면 스프링이 관리하는 세션에 저장됨:

```java
    model.addAttribute("loginId", "나의-엇썸한-아이디");
```

이렇게 SessionAttributes로 설정된 값은 `SessionStatus.setComplete()`로 세션을 초기화(서블릿 세션을 날리는게 아님)하기 전까지는 아래처럼 꺼낼 수 있다:

```java
  @RequestMapping("/엇썸한-유알엘")
  public ModelAndView forkYou(@ModelAttribute("loginId") String loginId) throws Exception {
    logger.debug(loginId); // "나의-엇썸한-아이디"
  }
```

이걸 어떻게 활용하냐면,

## 시나리오
