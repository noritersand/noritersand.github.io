---
layout: post
date: 2023-01-17 17:10:43 +0900
title: '[Spring] 스프링 부트'
categories:
  - spring
tags:
  - java
  - spring
  - spring-boot
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
- 

#### 버전 정보

- 이 글 작성 시점은:
  - Spring Boot v2.7.3
  - Spring v5.3.22


## 개요

요즘 필수로 쓰이는 스프링 부트, 스프링 시큐리티에 대한 짤막한 정리 모음. 


## 스태틱 리소스

스프링 부트는 아래의 경로들에 있는 [스태틱 웹 리소스(CSS나 자바스크립트 등의 파일)를 자동으로 추가](https://spring.io/blog/2013/12/19/serving-static-web-content-with-spring-boot)한다고 한다:

- `/META-INF/resources/`
- `/resources/`
- `/static/`
- `/public/`

가령 `src/main/resources/static/robots.txt` 파일이 있다면, 이 파일은 빌드 시 `WEB-INF/classes/static/robots.txt` 경로에 위치하게 되며, `/robots.txt` 웹 주소로 접근할 수 있다.

❓ 그리고 이 경로들은 스프링 시큐리티의 관리 대상에서도 자동으로 제외되는 것으로 보인다.

만약 정해진 경로를 사용하고 싶지 않다면 Web MVC 프레임워크에서 제공하는 `WebMvcConfigurer`의 구현체에서 `addResourceHandlers()` 의 오버라이딩 메서드를 통해 원하는 경로를 추가하는 방법이 있다:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/foo/**")
                .addResourceLocations("classpath:/foo/");
    }
}
```

`/foo/**`에 해당하는 웹 요청을 받으면 클래스패스의 `/foo/`에서 해당 자원을 찾으라는 설정이다. 테스트 하려면 `src/main/resources/foo/test.js`를 만들어놓고 웹에서 `/foo/test.js`를 요청해보자.

스태틱 리소스에 대한 공식 문서 링크:

- [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web.servlet.spring-mvc.static-content](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web.servlet.spring-mvc.static-content)
- [https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-static-resources](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-static-resources)


### StaticResourceLocation

추가 스프링 부트는 아래 웹 요청 경로들의 인증 절차를 생략한다:

- `/css/**`
- `/js/**`
- `/images/**`
- `/webjars/**`
- `/favicon.*`
- `/*/icon-*`

`org.springframework.boot.autoconfigure.security.StaticResourceLocation` 클래스에 정의되어 있으며, 이 경로들을 request matcher에서 사용하려면 이런식으로:

```java
PathRequest.toStaticResources().atCommonLocations()
```

`org.springframework.boot.autoconfigure.security.servlet.PathRequest`를 통해 가져오면 된다.


## 로컬 환경에서 서버 재시작 없이 JSP 리로드

외부 WAS를 사용할 때 그냥 되는 게 스프링 부트에선 안되는데, Spring Developer tools 라이브러리가 있어야 함:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>x.x.x</version>
</dependency>
```

debug/run 모드인지는 상관 없다.

구글링 하면 나오는 Live Reload는 별도의 훅과 브라우저의 확장 기능을 이용한 자동 새로고침 기능이다.


## TODO

```js
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return (web) -> web.ignoring()
                .mvcMatchers("/static-lib/**")
                .requestMatchers(PathRequest.toStaticResources().atCommonLocations());
    }
```

```java
    @Bean
    @Order(0)
    public SecurityFilterChain staticResourceFilterChain(HttpSecurity http) throws Exception {
        return http
                .requestMatchers(matchers -> matchers
                        //.requestMatchers(PathRequest.toStaticResources().atCommonLocations()) // 기본 정적 url
                        .antMatchers("/static-lib/**")
                        .antMatchers("/static-lib2/**")
                )
                .authorizeHttpRequests(authorize -> authorize
                        .anyRequest().permitAll())
                .requestCache(RequestCacheConfigurer::disable)
                .securityContext(AbstractHttpConfigurer::disable)
                .sessionManagement(AbstractHttpConfigurer::disable)
                .build();
    }
```
