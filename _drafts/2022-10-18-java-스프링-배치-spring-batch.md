---
layout: post
date: 2022-10-18 12:06:51 +0900
title: '[Java] 스프링 배치'
categories:
  - java
tags:
  - java
  - spring
  - batch
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://spring.io/projects/spring-batch#learn](https://spring.io/projects/spring-batch#learn)

#### 버전 정보

- 스프링 부트 2.6.2 기반으로 최초 작성된 글이지만, 오늘자(2022-10-18) 최신 버전은 4.3.7


## 개요

어쩌구


## 부트업 클래스

아래처럼 `@EnableBatchProcessing`, `@SpringBootApplication` 어노테이션이 적용된 클래스를 작성함:

```java
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@EnableBatchProcessing
@SpringBootApplication(scanBasePackages = {"net.mytest.batch"})
public class TestBatchApplication {
    public static void main(String[] args) {
        System.exit(SpringApplication.exit(SpringApplication.run(TestBatchApplication.class, args)));
    }
}
```


## 배치 실행 방법

### \#1 

부트업 클래스 대한 Run/Debug Configuration(이하 실행 설정)을 작성

### \#2 

실행 설정에서 Active profiles를 `local`로 지정한다. Active profiles가 안보이면 Modify options를 눌러서 추가하면 됨.

### \#3 

특정 배치만 실행하려면 실행 설정에서 아래처럼 Program Arguments 추가:
  
```java
--spring.batch.job.names=JOB_NAME
```

`JOB_NAME`은 `JobBuilderFactory`의 `.get()`으로 넘기는 문자열과 일치해야 함. 예를 들어 아래의 경우:

```java
@Slf4j
@Configuration
@RequiredArgsConstructor
public class MailJobConfig {
    // ...

    @Bean
    public Job mailJob(){
        return jobBuilderFactory
                .get("mailJob2") 
        // ...
```

`JOB_NAME`은 `"mailJob"` 



## Program arguments 전달 방법

가령 program arguments가 `requestDate=20220128`일 때, 아래처럼:

```java
import org.springframework.batch.core.configuration.annotation.JobScope;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

// ...

@Configuration
public class PointJobConfig {
    // ...
    
    @Bean
    public Job pointJob() {
        return jobBuilderFactory
                .get("pointJob")
                .preventRestart()
                .start(pay(null))
                .incrementer(new RunIdIncrementer())
                .build();
    }
    
    @JobScope
    @Bean
    public Step pay(@Value("#{jobParameters['requestDate']}") String requestDate) {
        log.debug("requestDate"); // 20220128
        return null;
    }
    
    // ...
```

SPEL과 JobParameters를 이용해 받아온다.

이 때 Job step에 반드시 `@JobScope`와 `@Bean` 어노테이션이 있어야 함.


## 트랜잭션 적용 방법

TODO

트랜잭션 적용에 추가 설정을 필요하지 않아 보이며, Tasklet 메서드 단위로 브랜잭션이 생성되는 걸로 확인됨.
