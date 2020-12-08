---
layout: post
date: 2013-08-17 21:26:00 +0900
title: '[Spring] Spring Framework: 자주 쓰는 annotation 정리 #1'
categories:
  - spring
tags:
  - java
  - spring
  - annotation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/)
- [http://docs.spring.io/spring/docs](http://docs.spring.io/spring/docs)

이 글은 스프링 2.x 기준으로 작성되었음.

사용빈도가 높은 어노테이션 위주로 정리.

목차에 없는 항목은 API 문서를 참고할 것. 구글링하는게속편한건함정

## @Component

- 패키지: org.springframework.stereotype
- 버전: spring 2.5
- 설정 위치: 클래스 선언부 앞

`<context:component-scan>` 태그를 설정파일에 추가하면 해당 어노테이션이 적용된 클래스를 빈으로 등록하게 된다. 범위는 디폴트로 singleton이며 `@Scope`를 사용하여 지정할 수 있다.

사용하려면 XML 설정파일에 `<context:component-scan>`을 정의하고 적용할 기본  패키지를 base-package 속성으로 등록한다.

`context:annotation-config` 태그는 어노테이션과 관련해서 다음의 BeanPostProcessor를 함께 등록 한다.

- `@Required(RequiedAnnotationBeanPostProcessor)`
- `@Autowired(AutowiredAnnotationBeanPostProcessor)`
- `@Resource`, `@PostConstruct`, `@PreDestory(CommonAnnotationBeanPostProcessor)`
- `@Configuration(ConfigurationClassPostProcessor)`

그 외 Repository, Service, Controller 포함
예를 들어 다음처럼 설정하면:

```xml
<context:component-scan base-package="xxx"/>
```

xxx 패키지 하위에 `@Component`로 선언된 클래스를 bean으로 자동 등록한다. bean의 이름은 해당 클래스명(첫글자는 소문자)이 사용된다.

`<context:component-scan />` 요소에는 scoped-proxy 속성이 존재 한다. scoped-proxy는 `<aop:scoped-poxy/>`처럼 WebApplicationContext 에서만 유효하며 "session", "globalSession", "request" 이외의 scope는 무시 되며 아래의 3가지 값을 설정 할 수 있다.

- no: proxy를 생성하지 않는다.(기본값)
- interfaces: JDK Dynamic Proxy를 이용한 Proxy 생성
- targetClass: 클래스에 대해 프록시를 생성(CGLIB를 이용한 Proxy 생성)

```java
@Component
@Scope("prototype")   // 생략하면 싱글톤
public class Test {
    // .....
}
```

### CGLIB

기존의 자바 클래스파일로부터 자바의 소스코드를 동적으로 생성하는 라이브러리(자바 소스 변경)

http://sourceforge.net/projects/cglib/

### 스캔 대상 클래스 범위 지정하기

`<context:include-filter>` 태그와 `<context:exclude-filter>` 태그를 사용하면 자동 스캔 대상에 포함시킬 클래스와 포함시키지 않을 클래스를 구체적으로 명시할 수 있다.

```xml
<context:component-scan base-package="spring.demo" scoped-proxy="no">
   <context:include-filter type="regex" expression="*HibernateRepository"/>
   <context:exclude-filter type="aspectj" expression="..*IBatisRepository"/>
</context:component-scan>
```

위와 같이 `<context:include-filter>` 태그와 `<context:exclude-filter>` 태그는 각각 type 속성과 expresseion 속성을 갖는데, type 속성에 따라 expression 속성에 올 수 있는 값이 달라진다. type 속성에 입력가능한 값은 다음과 같다:

- annotation: 클랙스에 지정한 어노테이션이 적용됐는지의 여부. expression 속성에서는 "org.example.SomeAnnotation"와 같은 어노테이션 이름을 입력한다.
- assignable: 클래스가 지정한 타입으로 할당 가능한지의 여부.  expression 속성에는 `org.exampleSomeClass` 와 같은 타입 이름을 입력한다.
- regex: 클래스 이름이 정규 표현식에 매칭되는 지의 여부.  expression 속성에는 `org\.example\.Default.*` 와 같이 정규 표현식을 입력한다.
- aspectj: 클래스 이름이 AspectJ 의 표현식에 매칭되는 지의 여부.  expression 속성에는 `org.example..*Service+` 와 같이 AspectJ 의 표현식을 입력한다.

## @Required

- 패키지: org.springframework.beans.factory.annotation
- 버전: spring 2.0
- 설정 위치: setter 메서드 앞

Required 어노테이션은 필수 프로퍼티임을 명시하는 것으로 필수 프로퍼티를 설정하지 않을 경우 빈 생성시 예외를 발생시킨다.

```java
import org.springframework.beans.factory.annotation.Required

public class TestBean {
    @Required
    private TestDao testDao;

    public void setTestDao(TestDao testDao) {
        this.testDao = testDao;
    }
}
```

```xml
<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanpostProcessor"/>
<bean name="testBean"  class="han.test.TestBean">
    <property name="testDao" ref="testDao"/>
    <!-- @Required 어노테이션을 적용하였으므로 설정하지 않으면 예외를 발생시킨다. -->
</bean>
```

RequiredAnnotationBeanPostProcessor 클래스는 스프링 컨테이너에 등록된 bean 객체를 조사하여 @Required 어노테이션으로 설정되어 있는 프로퍼티의 값이 설정되어 있는지 검사한다.

사용하려면 `<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor" />` 클래스를 빈으로 등록시켜줘야 하지만 이를 대신하여 `<context:annotation-config>` 태그를 사용해도 된다:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
             http://www.springframework.org/schema/context
             http://www.springframework.org/schema/context/spring-context-3.1.xsd">
    <context:annotation-config/>
</beans>
```

## @Autowired

- 패키지: org.springframework.beans.factory.annotation
- 버전: spring 2.5
- 설정 위치: 생성자, 필드, 메서드(setter메서드가 아니여도 된다) 앞

의존관계를 자동설정할 때 사용하며 타입을 이용하여 의존하는 객체를 삽입해 준다. 그러므로 해당 타입의 빈객체가 존재하지 않거나 또는 2개 이상 존재할 경우 스프링은 예외를 발생시키게 된다.

#### options

- required: Autowired 어노테이션을 적용한 프로퍼티 중 반드시 설정할 필요가 없는 경우에 false값을 주어 프로퍼티가 존재하지 않더라도 스프링이 예외를 발생하지 않도록 한다. 기본값은 TRUE. ex) @Autowired(required=false)

사용하려면 `<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor" />` 클래스를 빈으로 등록시켜줘야 한다. 해당 설정 대신에 `<context:annotation-config>` 태그를 사용해도 된다.

`@Autowired`를 적용할 때 같은 타입의 빈이 2개 이상 존재하게 되면 예외가 발생하는데, Autowired도 이러한 문제가 발생한다. 이럴 때 `@Qualifier`를 사용하면 동일한 타입의 빈 중 특정 빈을 사용하도록 하여 문제를 해결할 수 있다.

```java
@Autowired
@Qualifier("test")
private Test test;
```

## @Qualifier

- 패키지: org.springframework.beans.factory.annotation
- 버전: spring 2.5
- 설정 위치: `@Autowired` 어노테이션과 함께 사용된다.

qualifier 어노테이션은 `@Autowired`의 목적에서 동일 타입의 빈객체가 존재시 특정빈을 삽입할 수 있게 설정한다. `@Qualifier("mainBean")`의 형태로 `@Autowired`와 같이 사용하며 해당 `<bean>`태그에 `<qualifire value="mainBean" />` 태그를 선언해주어야 한다. 메서드에서 두개이상의 파라미터를 사용할 경우는 파라미터 앞에 선언해야한다.

#### options

- name: alias명

사용하려면 동일타입의 빈객체 설정에서 `<qualifier value="[alias명]" />`를 추가해 준다.

```xml
<bean id="user2" class="com.sp4.UserImpl">
    <property name="name" value="스프링"/>
    <property name="age" value="20"/>
    <property name="tel" value="000-0000-0000"/>
</bean>

<bean id="userService1" class="com.sp4.UserService"/>
```

```java
public class UserService {
    @Autowired
    @Qualifier("user2")
    private User user;

    public String result() {
        return user.getData();
    }
}
```

## @Resource
자바 6 및 JEE5에 추가된 것으로 애플리케이션에서 필요로 하는 자원을 자동 연결할 때 사용 한다. 스프링 2.5 부터 지원하는 어노테이션으로 스프링에서는 의존하는 빈 객체를 전달할 때 사용한다.

`@Autowired`와 흡사하지만 `@Autowired`는 타입으로(by type), `@Resource`는 이름으로(by name)으로 연결한다는 점이 다르다.

#### options

- name: 자동으로 연결될 빈객체의 이름을 입력한다. ex) `@Resource(name="testDao")`

사용하려면 `<bean class="org.springframework.beans.factory.annotation.CommonAnnotationBeanPostProcessor"/>` 클래스를 빈으로 등록시켜줘야 한다. 해당 설정 대신에 `<context:annotation-config>` 태그를 사용해도 된다.

```xml
<beans>
    <!-- 기타 설정 생략 -->
    <context:annotation-config/>

    <bean id="user2" class="com.test.UserImpl" p:data="65536"/>
</beans>
```

```java
public class UserService {
    @Resource(name="user2")
    private User user;
    //UserImpl user2 = new UserImpl();
    //User user = user2;

    public void setUser(User user) {
        this.user = user;
    }
    public String result() {
        return user.getData();
    }
}
```

## @Scope

- 패키지: org.springframework.beans.factory.annotation
- 설정: prototype, singleton, request, session, globalSession

스프링은 기본적으로 빈의 범위를 "singleton" 으로 설정한다. "singleton" 이 아닌 다른범위를 지정하고 싶다면 `@Scope` 어노테이션을 이용하여 범위를 지정한다.

```java
@Component
@Scope(value="prototype")
public class Worker {}
```

```java
@Component
@Scope(value="prototype", proxyMode=ScopedProxyMode.TARGET_CLASS)
public class Worker {}
```

## @PostConstruct

- 패키지: javax.annotation
- 버전: jdk1.6, spring 2.5
- 설정 위치: 초기화 작업 수행 메서드 앞

의존하는 객체를 설정한 이후에 초기화 작업을 수행하기 위해 사용한다. 스프링에 의해 인스턴스가 생성된 후 어노테이션이 적용된 메서드가 호출된다. 사용하려면 CommonAnnotationBeanPostProcessor 클래스를 빈으로 등록시켜줘야 한다. `<context:annotation-config>` 태그로 대신할 수 있다.

```java
@PostConstruct
public void init() {
    System.out.println("객체 생성 후 내가 먼저 실행된다.");
}
```

## @PreDestroy

- 패키지: javax.annotation
- 버전: jdk1.6, spring 2.5
- 설정 위치: 해당 작업 메서드 앞

컨테이너에서 객체를 제거하기 전에 해야할 작업을 수행하기 위해 사용한다.

사용하려면 CommonAnnotationBeanPostProcessor 클래스를 빈으로 등록시켜줘야 한다. `<context:annotation-config>` 태그로 대신할 수 있다.

## @Inject

SR-330 표준 Annotation으로 Spring 3 부터 지원하는 Annotation이다. 특정 Framework에 종속되지 않은 애플리케이션을 구성하기 위해서는 `@Inject`를 사용할 것을 권장한다. `@Inject`를 사용하기 위해서는 클래스 패스 내에 JSR-330 라이브러리인 javax.inject-x.x.x.jar 파일이 추가되어야 함에 유의해야 한다.

## @Service
`@Service`를 적용한 Class는 비지니스 로직이 들어가는 Service로 등록이 된다. Controller에 있는 `@Autowired`는 `@Service("xxxService")`에 등록된 xxxService와 변수명이 같아야 하며 Service에 있는 `@Autowired`는 `@Repository("xxxDao")`에 등록된 xxDao와 변수명이 같아야 한다.

```java
@Service("helloService")
public class HelloServiceImpl implements HelloService {
    @Autowired
    private HelloDao helloDao;

    public void hello() {
        System.out.println("HelloServiceImpl :: hello()");
        helloDao.selectHello();
    }
}
```

`helloDao.selectHello()` 와 같이 `@Autowired`를 이용한 객체를 이용하여 Dao 객체를 호출한다:

```java
@Service("test2.testService")
//괄호 속 문자열은 식별자를 의미한다.
//괄호를 생략할 경우 클래스명 그대로 사용한다.
//따라서 같은 클래스명이 존재 할 시 같은 식별자가 생성되기때문에 에러가 발생한다.
public class TestService {
    public String result(int num1, int num2, String oper) {
        String str = null;

        if (oper.equals("+")) {
            //...
            return str;
        }
    }
}
```

@Resouce로 연결

```java
@Resource(name="test2.testService")
//name에 필요한 것은 @Service("test2.testService") <- 여기서 괄호 속 문자열, 즉 식별자

private TestService service;
//TestService service = new TestService(); 라고 하는것과 같은 식

@RequestMapping(value="/test2/oper.action", method={RequestMethod.GET})
public String form() throws Exception {
    return "test2/write";
}
```

## @Repository

- 패키지: org.springframework.stereotype
- 버전: spring 2.0

`@Repository`는 일반적으로 DAO에 사용되며 DB Exception을 DataAccessException으로 변환한다.

```java
@Repository("bbs.boardDAO")
public class BoardDAO {
    private SqlSession sqlSession;

    public int insertBoard(Board dto) throws Exception {
        ...
    }
}
```

```java
public class BoardServiceImpl implements BoardService {
    @Resource(name="bbs.boardDAO")
    private BoardDAO dao;

    public int insertBoard(Board dto){}
}
```

## @SessionAttributes

SessionAttribute annotation은 세션상에서 model의 정보를 유지하고 싶을 경우 사용한다.

```java
@Controller
@SessionAttributes("blog")
public class BlogController {
    // 중간생략

    @RequestMapping("/createBlog")
    public ModelMap createBlogHandler() {
        blog = new Blog();
        blog.setRegDate(new Date());
        return new ModelMap(blog);
    }

    // 중간생략
}
```

`@SessionAttributes`는 model로 할당된 객체 중 지정된 이름과 일치하는 객체를 세션 속성에도 추가한다.

```java
@SessionAttributes("someSessionAttr")
class ExampleController2 {
    @RequestMapping("/test")
    public ModelAndView drawTest(ModelAndView view, @ModelAttribute("someSessionAttr") String someSessionAttr) {
        view.addObject("someSessionAttr", someSessionAttr);
        return view;
    }
}
```

## @RequestBody

`@RequestBody` 어노테이션이 적용된 파라미터는 HTTP Request body의 내용이 전달된다.

참고: http://java.ihoney.pe.kr/283

```java
@RequestMapping(value="/test")
public void penaltyInfoDtlUpdate(@RequestBody String body,
        HttpServletRequest req, HttpServletResponse res,
        Model model, HttpSession session) throws Exception  {

    System.out.println(body);
}
```

## @ResponseBody

참고: http://ismydream.tistory.com/140

클라이언트에 JSON 형식의 값을 응답할 때 유용하다. 메서드에 `@ResponseBody`를 적용한 후 문자열을 리턴하면 그 값은 HTTP response header가 아니라 HTTP response body에 쓰여진다. 객체를 넘길경우 스프링에 내장된 JACKSON에 의해 문자열로 변환될 것이다.

또한 `@ResponseBody`가 적용된 컨트롤러는 context에 설정된 resolver를 무시한다.

```java
@RequestMapping("/getVocTypeList")
@ResponseBody
public ArrayList<Object> getVocTypeList() throws Exception {
    HashMap<String, Object> vocData = gvocInf.searchVocTypeList();
    return (ArrayList<Object>) vocData.get("data");
}
```
