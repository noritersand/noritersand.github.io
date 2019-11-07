---
layout: post
date: 2016-01-24 20:17:00 +0900
title: '[Spring] @Controller'
categories:
  - spring
tags:
  - java
  - spring
  - controller
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

-

패키지: org.springframework.stereotype

버전: spring 2.5

spring MVC의 Controller 클래스 선언을 단순화시켜준다. 스프링 컨트롤러, 서블릿을 상속할 필요가 없으며, `@Controller`로 등록된 클래스 파일에 대한 bean을 자동으로 생성해준다.

Controller로 사용하고자 하는 클래스에 `@Controller` 어노테이션을 명시하면 component-scan으로 자동 등록된다.

```xml
<context:component-scan base-package="com.*"/>
```

```java
package com.test;

import org.springframework.stereotype.Controller;

@Controller
public class SpringTest {
    //...
}
```

## 컨트롤러 메서드의 파라미터 타입

| 파라미터 타입 | 설명  |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
|  HttpServletRequest, HttpServletResponse, HttpSession |  Servlet API |
|  java.util.Locale |  현재 요청에 대한 Locale 정보 |
|  InputStream, Reader |  요청 컨텐츠에 직접 접근할 때 사용 |
|  OutputStream, Writer |  응답 컨텐츠를 생성할 때 사용 |
|  @PathVariable 어노테이션 적용 변수 |  URL 템플릿 변수에 접근할 때 사용 |
|  @RequestParam 어노테이션 적용 변수 |  HTTP 파라미터와 매핑 |
|  @RequestHeader 어노테이션 적용 변수 |  HTTP 헤더 매핑 |
|  @CookieValue 어노테이션 적용 변수 |  HTTP 쿠키 매핑 |
|  @RequestBody 어노테이션 적용 변수 |  HTTP RequestBody에 접근할 때 사용. HttpMessage Converter를 이용해 RequestBody 데이터를 해당 타입으로 변환한다. |
|  Map, Model, ModelMap |  뷰에 전달할 모델 데이터를 설정할 때 사용  |
|  커맨드 객체 |  HTTP 요청 파라미터를 저장한 객체, 기본적으로 클래스 이름을 모델명으로 사용. |
|  Errors, BindingResult |  HTTP 요청 파라미터를 커맨드 객체에 저장한 결과. 커맨드 객체를 위한 파라미터 바로 다음에 위치한다. |
|  SessionStatus |  폼 처리를 완료 했음을 처리하기 위해 사용. @SessionAttribute를 명시한 session 속성을 제거하도록 이벤트를 발생시킨다. |

### Servlet API의 HttpServletRequest, HttpServletResponse, HttpSession

```java
@RequestMapping(params = "param=add")
public String addProduct(HttpServletRequest request, HttpServletResponse,
        HttpSession session) throws Exception {
    // ....
}
```

### java.util.Locale

```java
@RequestMapping(params = "param=add")
public String addProduct(Locale locale, Product product,
        BindingResult result, SessionStatus status) throws Exception {
    // 중략

    String message = messageSource.getMessage(
    "product.error.exist", new String[] {product.getProductNo()}, locale);
}
```

### java.io.InputStream 또는 java.io.Reader

Request의 content를 직접 처리할 경우 (Servlet API가 제공하는 raw InputStream/Reader)

```java
@RequestMapping(params = "param=add")
public String addProduct(InputStream is, Product product,
        BindingResult result, SessionStatus status) throws Exception {
    // 중략

    for (int totalRead = 0; totalRead < totalBytes; totalRead += readBytes) {
        readBytes = is.read(binArray, totalRead, totalBytes - totalRead);
    }

    // 중략
}
```

### @RequestParam annotation이 적용된 argument

ServletRequest.getParameter(java.lang.String name)와 같은 역할 수행

```java
@RequestMapping("/deleteProduct.do")
public String deleteProduct(@RequestParam("productNo") String productNo) {
    productService.deleteProduct(productNo);
    return "/listProduct.do";
}
```

### java.util.Map 또는 org.springframework.ui.Model 또는 org.springframework.ui.ModelMap

Web View로 데이터를 전달해야 하는 경우 위 타입의 argument를 정의하고, 메서드 내부에서 View로 전달할 데이터를 추가함

```java
@RequestMapping("/getProduct.do")
public String getProduct(@RequestParam("productNo") String productNo, Map map) {
    Product product = productService.getProduct(productNo);        
    map.put("product", product);
    return "/jsp/annotation/sales/product/viewProduct.jsp";
}

@RequestMapping("/getProduct.do")
public String getProduct(@RequestParam("productNo") String productNo, Model model) {
    Product product = productService.getProduct(productNo);
    model.addAttribute("product", product);
    return "/jsp/annotation/sales/product/viewProduct.jsp";
}

@RequestMapping("/getProduct.do")
public String getProduct(@RequestParam("productNo") String productNo, ModelMap modelMap) {
    Product product = productService.getProduct(productNo);
    modelMap.addAttribute("product", product);
    return "/jsp/annotation/sales/product/viewProduct.jsp";
}
```

### Command 또는 form 객체

HTTP Request로 전달된 parameter를 binding한 객체로 다음 View에서 사용 가능하고 `@SessionAttributes`를 통해 session에 저장되어 관리될 수 있다. `@ModelAttribute annotation`을 이용하여 사용자 임의로 이름을 부여할 수 있다.

```java
@RequestMapping("/addProduct.do")
public String updateProduct(Product product, SessionStatus status)
        throws Exception {
    // 여기서 'product'가 Command(/form) 객체이다.
    return "/listProduct.do";
}

@RequestMapping("/addProduct.do")
public String updateProduct(@ModelAttribute("updatedProduct") Product product,
        SessionStatus status) throws Exception {
    // 여기서 'updatedProduct'라는 이름의 'product'객체가 Command(/form) 객체이다.
    return "/listProduct.do";
}
```

### org.springframework.validation.Errors 또는 org.springframework.validation.BindingResult

바로 이전의 입력파라미터인 Command 또는 form 객체의 validation 결과 값을 저장하는 객체로 해당 command 또는 form 객체 바로 다음에 위치해야 함에 유의하도록 한다.

```java
@RequestMapping(params = "param=add")
public String addProduct(HttpServletRequest request,
        Product product, BindingResult result, SessionStatus status) throws Exception {        
    new ProductValidator().validate(product, result);
    if (result.hasErrors()) {
        return "/jsp/annotation/sales/product/productForm.jsp";
    } else {
        // 중략
        return "/listProduct.do";
    }
}
```

### org.springframework.web.bind.support.SessionStatus

폼 처리가 완료되었을 때 status를 처리하기 위해서 argument로 설정. `SessionStatus.setComplete()`를 호출하면 컨트롤러 클래스에 `@SessionAttributes`로 정의된 Model객체를 session에서 지우도록 이벤트를 발생시킨다.

```java
@RequestMapping(params = "param=add")
public String addProduct(HttpServletRequest request,
        Product product, BindingResult result, SessionStatus status) {
    // 중략
    productService.addProduct(product);
    status.setComplete();
    return "/listProduct.do";
}
```

## 컨트롤러 메서드의 리턴 타입

| 리턴 타입 | 설명 |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  ModelAndView |  뷰 정보 및 모델 정보를 담고 있는 ModelAndView 객체  |
|  Model |  뷰에 전달할 객체 정보를 담고 있는 Model을 리턴한다. 이때 뷰 이름은 요청 URL로부터 결정된다. (RequestToViewNameTranslator를 통해 뷰 결정)  |
|  Map, ModelMap |  뷰에 전달할 객체 정보를 담고 있는 Map 혹은 ModelMap을 리턴한다. 이때 뷰 이름은 요청 URL로부터 결정된다. (RequestToViewNameTranslator를 통해 뷰 결정)  |
|  String |  뷰 이름을 리턴한다.  |
|  View 객체 |  View 객체를 직접 리턴. 해당 View 객체를 이용해서 뷰를 생성한다.  |
|  void |  메서드가 ServletResponse나 HttpServletResponse 타입의 파라미터를 갖는 경우 메서드가 직접 응답을 처리한다고 가정한다. 그렇지 않을 경우 요청 URL로부터 결정된 뷰를 보여준다. (RequestToViewNameTranslator를 통해 뷰 결정)  |
|  `@ResponseBody` 어노테이션 적용 |  메서드에서 `@ResponseBody` 어노테이션이 적용된 경우, 리턴 객체를 HTTP 응답으로 전송한다. HttpMessageConverter를 이용해서 객체를 HTTP 응답 스트림으로 변환한다.  |

### ModelAndView 객체

View와 Model 정보를 모두 포함한 객체를 리턴하는 경우

```java
@RequestMapping(params = "param=addView")
public ModelAndView addProductView() {
    ModelAndView mnv = new ModelAndView("/jsp/product/productForm.jsp");
    // mnv.setViewName("/jsp/product/productForm.jsp");
    mnv.addObject("product", new Product());
    return mnv;
}
```

### Map, ModelMap

Web View로 전달할 데이터만 리턴하는 경우

```java
@RequestMapping("/productList.do")
public Map getProductList() {
    List productList = productService.getProductList();
    ModelMap map = new ModelMap(productList); //productList가 "productList"라는 이름으로 저장됨.
    return map;
}
```

여기서 View에 대한 정보를 명시적으로 리턴하지는 않았지만, 내부적으로 View name은 RequestToViewNameTranslator에 의해서 입력된 HTTP Request를 이용하여 생성된다. 예를 들어 DefaultRequestToViewNameTranslator 는 입력된 HTTP Request URI를 변환하여 View name을 다음과 같이 생성한다.

- http://localhost:8080/test/display.do -> 생성된 View name: 'display'
- http://localhost:8080/test/admin/index.do -> 생성된 View name: 'admin/index'

위와 같이 자동으로 생성되는 View name에 'jsp/'와 같이 prefix를 붙이거나 '.jsp' 같은 확장자를 덧붙이고자 할 때는 아래와 같이 속정 정의 XML(xxx-servlet.xml)에 추가하면 된다.

```xml
<bean id="viewNameTranslator"
        class="org.springframework.web.servlet.view.DefaultRequestToViewNameTranslator">
    <property name="prefix" value="jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

### Model

Web View로 전달할 데이터만 리턴하는 경우 Model은 Java5 이상에서 사용할 수 있는 인터페이스이다. 기본적으로 ModelMap과 같은 기능을 제공한다. Model 인터페이스의 구현클래스에는 BindingAwareModelMap 와 ExtendedModelMap 이 있다. View name은 위에서 설명한 바와 같이 RequestToViewNameTranslator에 의해 내부적으로 생성된다.

```java
@RequestMapping("/productList.do")
public Model getProductList() {
    List productList = productService.getProductList();
    ExtendedModelMap map = new ExtendedModelMap();
    map.addAttribute("productList", productList);
    return map;
}
```

### String

View name만 리턴하는 경우:

```java
@RequestMapping(value = {"/addProduct.do", "/updateProduct.do" })
public String updateProduct(Product product, SessionStatus status) throws Exception {
    return "/listProduct.do";
}
```

### void

메서드 내부에서 직접 HTTP Response를 직접 처리하는 경우. 또는 View name이 RequestToViewNameTranslator에 의해 내부적으로 생성되는 경우:

```java
@RequestMapping("/addView.do")
public void addView(HttpServletResponse response) {
    // 중략
    // response 직접 처리
}

@RequestMapping("/addView.do")
public void addView() {
    // 중략
    // View name이 DefaultRequestToViewNameTranslator에 의해서 내부적으로 'addView'로 결정됨.
}
```

### @ResponseBody

메서드에서 `@ResponseBody` 어노테이션이 적용된 경우 해당 값을 HTTP ResponseBody로 전송한다. HttpMessageConverter를 이용해서 객체를 HTTP 응답 스트림으로 변환한다.
