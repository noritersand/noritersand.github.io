---
layout: post
date: 2013-10-21 17:14:00 +0900
title: 'Spring: 트랜잭션 관리 managing transactions'
categories:
  - spring
tags:
  - java
  - spring
  - transactional
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://spring.io/guides/gs/managing-transactions/](http://spring.io/guides/gs/managing-transactions/)
- [http://wiki.gurubee.net/pages/viewpage.action?pageId=26741432](http://wiki.gurubee.net/pages/viewpage.action?pageId=26741432)

## configuration

설정은 아래 참고

- [http://docs.spring.io/spring/docs/current/spring-framework-reference/html/transaction.html](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/transaction.html)
- [http://blog.outsider.ne.kr/870](http://blog.outsider.ne.kr/870)

## @Transactional Annotation을 이용한 처리

클래스 혹은 메서드 선언부에 `@Transactional` 어노테이션을 명시하면 해당 클래스의 프록시가 생성되며 이후 예외가 발생하면 프록시에 의해 자동으로 롤백 된다.

```java
@Service
public class TransactionSample {
    @Autowired(required = true)
    private BoardDao boardDao;

    @Transactional
    public int updateBoard(BoardModel[] modelArray) throws Exception {
        int result = 0;

        for (BoardModel ele: modelArray) {
            result += boardDao.updateBoard(ele);
        }

        return result;
    }
}
```

`@Transactional` 어노테이션이 없는 메서드에서 같은 클래스 내에 있는 `@Transactional` 메서드를 호출할 때엔 트렌젝션이 제대로 적용되지 않으니 주의해야 한다. [관련 글 링크.](http://stackoverflow.com/questions/3423972/spring-transaction-method-call-by-the-method-within-the-same-class-does-not-wo). 이것은 스프링이 `@Transactional`을 처리하기 위해 동적으로 생성하는 프록시의 특성 때문이다. 동적 생성된 프록시는 어노테이션이 있는 메서드를 제외하곤 모두 원본이 되는 메서드를 delegate 형식으로 오버라이드 하고 있는데 이 delegate 메서드를 직접 호출해버리면 프록시가 아닌 원본 객체를 경유하기 때문에 프록시 내에 구현된 트렌젝션 처리를 무시해버리는 현상이라고 보면 된다.

## DataSourceTransactionManager를 이용한 처리

메서드 본문에서 메서드 본문 전체가 아닌 본문 중 특정 지역별로 트랜잭션을 묶어야 할 때 사용한다.

```java
@Service
public class TransactionSample {
    @Autowired(required = true)
    private DataSourceTransactionManager transactionManager;

    @Autowired(required = true)
    private BoardDao boardDao;

    public int updateBoard(BoardModel[] modelArray) {
        DefaultTransactionDefinition def = new DefaultTransactionDefinition();
        def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
        TransactionStatus status = transactionManager.getTransaction(def);

        int result = 0;

        try {
            for (BoardModel ele: modelArray) {
                result += boardDao.updateBoard(ele);
            }
            transactionManager.commit(status);
        } catch (Exception e) {
            transactionManager.rollback(status);
            throw e;
        }

        return result;
    }
}
```
