---
layout: post
date: 2022-08-25 18:07:03 +0900
title: '[Java] SessionBucket'
categories:
  - java
tags:
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 버전 정보

- x.x.x

## 설명

HttpSession의 `setAttribute()`를 직접 호출하는게 마음에 안들어서 만들어 본 중간 클래스.

필요한 이유: session은 리퀘스트와 달리 전 영역에 걸쳐 하나의 주소에 set/get이 가능하기 때문에 관리와 리펙토링이 쉽도록 중간자 역할을 하는 녀섴이 필요함.

반면 HttpServletRequest는 중간 클래스 필요성이 적은데, request scope는 session에 비해 범위가 훨씬 좁기 때문.

## 코드

```java
import lombok.Data;
import lombok.Getter;

import javax.servlet.http.HttpSession;

/**
 * <p>HTTP Session의 attribute를 다루는 중간 클래스.
 * <p>setAttribute(), getAttribute()를 직접 하는 건 좋지 않아 만듬
 *
 * @author fixalot
 * @since 2022-08-25
 */
@Getter
public class SessionBucket {
    private static final String KEY = "sessionBucket";

    public SessionBucket(HttpSession session) {
        session.setAttribute(KEY, this);
    }

    public static SessionBucket getInstance(HttpSession session) {
        SessionBucket instance = (SessionBucket) session.getAttribute(KEY);
        if (instance == null) {
            throw new IllegalArgumentException("인스턴스를 찾을 수 없어요. 이미 제거됐거나 생성자를 호출하지 않았습니다.");
        }
        return instance;
    }

    public static void clear(HttpSession session) {
        session.removeAttribute(KEY);
    }

    private SomeObject someObject = new SomeObject();

    @Data
    public static class SomeObject {
        private boolean flag;
        private String str;
        private Integer num;
    }
}
```

`SomeObject` 같은 내부 클래스는 데이터의 카테고리 역할로 사용하면 될 것 가틈.

끗.