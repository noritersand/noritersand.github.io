---
layout: post
date: 2015-04-13 18:58:00 +0900
title: '[Spring] @ModelAttribute'
categories:
  - spring
tags:
  - java
  - spring
  - modelattribute
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://docs.spring.io/autorepo/docs/spring/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html](http://docs.spring.io/autorepo/docs/spring/current/javadoc-api/org/springframework/web/bind/annotation/ModelAttribute.html)
- [http://hnsnmn.blogspot.kr/2014/02/modelattribute.html](http://hnsnmn.blogspot.kr/2014/02/modelattribute.html)
- [http://kunner.tistory.com/998](http://kunner.tistory.com/998)

#### since

- spring framwork 2.5


## 파라미터에 적용할 때

```java
method( @ModelAttribute Model model )
method( @ModelAttribute( name ) Model model )
```

- `name`: attribute name

화면에서 전달된 쿼리스트링이나 폼 데이터를 모델에 자동 할당하는 기능은 `@ModelAttribute` 어노테이션 없이도 작동한다. `@ModelAttribute`는 데이터가 바인딩된 객체를 VIEW에서 재사용되어야 할 필요가 있을 때 사용한다.

가령 다음의 경우엔 MyModel이 "specified"란 이름으로 VIEW에 전달된다:

```java
@RequestMapping("/some.do")
public ModelAndView draw(@ModelAttribute("specified") MyModel myModel) {
    // ...
}
```

이후 VIEW에선 'specified'를 request.attribute에서 가져올 수 있다. 단, `@RequestParam`과 같이 사용할 땐 이 기능이 작동하지 않는걸로 보인다.

## 메서드에 적용할 때

```java
@ModelAttribute returnType methodName( ... )
```

컨트롤러에서 뷰에 전달할 일종의 공통 모델을 설정한다. 메서드가 리턴하는 값은 Request객체에 전달되며 전달 범위는 해당 메서드가 존재하는 컨트롤러 전체에 해당된다. `@ModelAttribute` 어노테이션이 적용된 메서드는 컨트롤러가 자동으로 호출한다. 단, 매 요청마다 반복 실행되므로 효율성을 고려해야 한다.

```java
@Controller
public class TestController {
    @RequestMapping("test/viewTest.do")
    public ModelAndView viewTest() {
        ModelAndView mv = new ModelAndView();
        mv.setViewName("/test/viewTest");
        return mv;
    }

    @ModelAttribute("myObject")
    public String refModelTest() {
        return "hello-o";
        // request.setAttirubte("myObject", "hello-o") 와 같으며
        // 선언된 클래스의 전역으로 실행된다.
    }
}
```
