---
layout: post
date: 2015-09-18 18:55:00 +0900
title: 'Java: java.lang.ThreadLocal'
categories:
  - java
tags:
  - java
  - threadlocal
---

* Kramdown table of contents
{:toc .toc}

```java
public static final ThreadLocal<String> LOCAL_VARIABLE = new ThreadLocal<String>() {
    @Override
    protected String initialValue() {
        return "hello"; // LOCAL_VARIABLE은 매 쓰레드가 종료되면(시작할떄?) 'hello'로 초기화된다.
    }
}
```

```java
    public static void main(String[] args) {
        LOCAL_VARIABLE.set("my new string");
    }
```

```java
    private static final ThreadLocal<String> LOCAL = new ThreadLocal<String>();

    @RequestMapping("/member/member.do")
    public ModelAndView drawMember(ModelAndView mv) {
        System.out.println(LOCAL.get()); // 매번 null
        LOCAL.set("1234");
        System.out.println(LOCAL.get()); // 1234

    // 이하 생략
```

ThreadLocal 변수: 쓰레드가 종료되면 초기화되는 변수

ThreadLocal 타입에 static이나 final이 없어도 작동에 문제 없는듯? 확인할 것.

## ecbase sample

```java
public class ThreadVariableRegistry {
    /** Thread의 Reference를 키값으로 하는 Map */
    private static Map<Thread, Map<String, Object>> registry = new HashMap<Thread, Map<String, Object>>();

    /** Thread 에서 참조할 값을 지니는 thread local 변수 */
    private static ThreadLocal<String> threadLocalCache = new ThreadLocal<>();

    /** 현재 쓰레드의 contextId 키 */
    public static final String CONTEXT_ID = "contextId";
    public static final String CLIENT_ID = "clientId";
    public static final String CLIENT_TRANSACTION_ID = "clientTransactionId";
    public static final String TRANSACTION_ID = "transactionId";
    public static final String BROWSER_IP = "browserIp";
    public static final String REQURESTED_HOST = "requestedHost";
    public static final String PROGRAM_ID = "programId";

    /** 싱글 쓰레드일 경우 사용하는 contextId 값 */
    public static final String DUMMY_CONTEXT = "dummyContext";
    public static final String USER_ID = "userId";

    /**
     * (현재 쓰레드의 레퍼런스) + (key) 를 키 값으로 value를 저장한다.
     *
     * @param key
     * @param value
     */
    public static void put(String key, Object value) {
        Map<String, Object> threadRegistry = registry.get(Thread.currentThread());
        if (threadRegistry == null) {
            threadRegistry = new HashMap<String, Object>();
            registry.put(Thread.currentThread(), threadRegistry);
        }
        threadRegistry.put(key, value);
    }

    /**
     * (현재 쓰레드의 레퍼런스) + (key) 를 키 값에 저장된 value를 리턴한다.
     *
     * @param key
     * @return
     */
    public static Object get(String key) {
        Map<String, Object> threadRegistry = registry.get(Thread.currentThread());
        return (threadRegistry == null) ? null : threadRegistry.get(key);
    }

    /**
     * Thread local 변수에 값을 입력한다.
     *
     * @param value
     */
    public static void setLocalCache(String value) {
        threadLocalCache.set(value);
    }

    /**
     * Thread local 변수의 값을 반환한다.
     *
     * @return
     */
    public static String getLocalCache() {
        return threadLocalCache.get();
    }
}
```

이런 놈을...

```java
public class AuthInterceptor extends HandlerInterceptorAdapter {
    public boolean preHandle(HttpServletRequest req, HttpServletResponse res, Object handle) throws Exception {

        // 기타 전처리 생략

        ThreadVariableRegistry.put(ThreadVariableRegistry.USER_ID, "fixalot");
        ThreadVariableRegistry.put(ThreadVariableRegistry.PROGRAM_ID, "프로그램ID");

        return true; // or false
    }
}
```

이렇게 설정해 두고

```java
public class BaseModel {
    /** 생성자 관리 번호 */
    private String registrantId;
    /** 생성 프로그램 ID */
    private String registerProgramId;

    public String getRegistrantId() {
        if (StringUtils.isNotEmpty(registrantId)) {
            return registrantId;
        } else {
            return this.getUserId();
        }
    }

    public String getUserId() {
        if (ThreadVariableRegistry.get(ThreadVariableRegistry.USER_ID) == null) {
            return "";
        }
        return ThreadVariableRegistry.get(ThreadVariableRegistry.USER_ID).toString();
    }

    public String getRegisterProgramId() {
        if (StringUtils.isNotEmpty(registerProgramId)) {
            return registerProgramId;
        } else {
            return this.getProgramId();
        }
    }

    public String getProgramId() {
        if (ThreadVariableRegistry.get(ThreadVariableRegistry.PROGRAM_ID) == null) {
            return "";
        }
        return ThreadVariableRegistry.get(ThreadVariableRegistry.PROGRAM_ID).toString();
    }
}
```

요딴식으로 씀
