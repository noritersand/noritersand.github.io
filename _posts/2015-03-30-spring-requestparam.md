---
layout: post
date: 2015-03-30 17:23:00 +0900
title: '[Spring] @RequestParam'
categories:
  - spring
tags:
  - java
  - spring
  - bindexception
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html)

#### required

- spring framework 2.5 or higher

RequestParam annotation은 `key=value` 형태로 화면에서 넘어오는 쿼리스트링 혹은 폼 데이터를 메서드의 매개변수로 지정한다.

```java
method( @RequestParam( PARAM ) Obj )
method( @RequestParam Map)
```

- `param`: 전달되는 파라미터의 이름을 지정한다. 이름 외에 기본값(defaultValue), 필수여부(required)를  설정할 수 있다. 값이 할당될 변수의 타입이 Map 혹은 MultiValueMap일 땐 명시하지 않는다.
- `obj`: PARAM으로 지정된 이름과 일치하는 파라미터의 값을 할당할 변수. 보통 String 타입을 선언하지만 넘  어온 값이 반드시 숫자일 경우에 한해서 int 등의 숫자 타입도 가능하다.

## 이름과 변수를 지정하는 방식

아래에서 `xxx/editBlog.do?blogId=3` 과 같이 접근할 때, editBlogHandler 메서드의 매개변수인 blogId에는 3이 셋팅된다. 필수 요건이 아닐 경우, `@RequestParam(value="id", required="false")`와 같이 옵션을 주고 사용할 수 있다.

```java
@Controller
public class BlogController {

    @RequestMapping("/editBlog")
    public ModelMap editBlogHandler(@RequestParam("blogId") int blogId) {
        blog = blogService.findBlog(blogId);
        return new ModelMap(blog);
    }

    // ...
}
```

```java
@RequestMapping(value="/...", method={RequestMethod.GET, RequestMethod.POST})
public String submit(HttpServletRequest req,
        @RequestParam(value="num1") int num1,
        @RequestParam(value="num2") int num2,
        @RequestParam(value="oper") String oper) throws Exception {
    // value: request parameter의 이름

    // 생략
}

//@RequestParam 어노테이션이 적용된 파라미터는 기본적으로 필수 파라미터이다.
//따라서, 명시한 파라미터가 존재하지 않을 경우 400 에러가 발생한다.
//여기서 파라미터에 값이 있을수도 없을수도 있는 로직을 구현하려면 다음처럼 작성한다.

@RequestMapping(value="/...", method={RequestMethod.GET, RequestMethod.POST})
public String submit(HttpServletRequest req,
        @RequestParam(value="num1", defaultValue = "0") int num1,
        @RequestParam(value="num2", defaultValue = "0") int num2,
        @RequestParam(value="oper", required=false) String oper)
        throws Exception {

    // 생략
}
```

## Map 방식

값을 할당할 변수의 타입을 Map 혹은 MultiValueMap으로 사용하는 방법.

```java
@RequestMapping("/faqDetail")
public String faqDetail(@RequestParam HashMap<String, String> map) {
    String searchValue = map.get("searchValue");
    // req.getParameter("searchValue") 와 같다.

    return "/board/faq/faqDetail";
}
```
