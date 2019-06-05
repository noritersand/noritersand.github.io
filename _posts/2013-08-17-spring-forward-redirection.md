---
layout: post
date: 2013-08-17 19:55:00 +0900
title: 'Spring: forward/redirection'
categories:
  - spring
tags:
  - java
  - spring
  - forward
  - redirection
---

* Kramdown table of contents
{:toc .toc}

스프링 프레임워크에서 컨트롤러의 메서드가 리턴하는 타입에 따라 forward와 redirect 구현 방법을 간단히 기술한다. 단, 지원되는 resolver는 설정되어 있다고 가정 따로 언급하지 않는다.

## return String

```java
return "/member/login.do"; // request forward
return "redirect:/member/login.do"; // request redirect
```

리디렉트땐 `redirect:` 이후 꺽쇠`/`의 여부에 따라 클라이언트에 전달할 경로가 달라질 수 있다. 가령 Context 경로가 `/FO`이고 컨트롤러에 매핑된 경로의 최상단(`/FO` 바로 다음)이 `/member`라고 했을 때 `redirect:member/login.do` 를 리턴하면 실제 전달되는 경로는 '/bo/member/member/login.do' 가 된다.

## return ModelAndView

```java
ModelAndView mv = new ModelAndView();
mv.setViewName("/member/memberDetail");
mv.addAttribute("msg", helloService.getMessage());
//mav.addObject("msg", helloService.getMessage());
return mav;

// 혹은
// req.setAttribute("msg", helloService);
// return new ModelAndView("/member/memberDetail");
```

```java
ModelAndView mv = new ModelAndView();
mv.setViewName("redirect:/member/memberDetail.do?memberNumber=2");
return mv;

// 혹은
// ModelAndView mv = new ModelAndView();
// mv.setView(new RedirectView("/member/memberDetail.do?memberNumber=2", true));
// return mv;
```
