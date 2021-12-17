---
layout: post
date: 2019-03-06 18:24:00 +0900
title: '[Java] static import'
categories:
  - java
tags:
  - java
  - import
  - static-import
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://docs.oracle.com/javase/7/docs/technotes/guides/language/static-import.html](https://docs.oracle.com/javase/7/docs/technotes/guides/language/static-import.html)
- [https://docs.oracle.com/javase/specs/jls/se7/html/jls-7.html#jls-7.5.3](https://docs.oracle.com/javase/specs/jls/se7/html/jls-7.html#jls-7.5.3)

#### static import

```java
import static org.junit.Assert.*;
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertTrue;

public class ImportTest {

    public void test() {
        assertEquals(1, 2);
        assertTrue(false);
    }
}
```

특정 클래스의 스태틱 멤버를 접근연산자 없이 호출할 수 있게 하는 import문의 일종이다. 위 코드에서 첫 번째 줄은 `org.junit.Assert` 클래스의 모든 스태틱 멤버를 import한다는 뜻이다. 따라서 둘 째, 셋 째줄은 무쓸모한 코드다.


#### 이클립스를 사용한다면

아래 코드는:

```java
import static org.junit.Assert.*;
```

이클립스의 설정이 기본값일 때 'Organize Imports'를 실행하면 날아간다. 이 현상을 방지하려면 `Windows` > `Preferences` > `Java` > `Code Style` > `Organize Imports`에서 `Number of static imports needed for .*`를 1로 변경한다.

만약 이 값이 4라면, static import가 4개 미만일 때는 아래처럼 스태틱 멤버를 각각 import하며:

```java
import static laboratory.constants.Const.WHO;
import static laboratory.constants.Const.WHERE;
import static laboratory.constants.Const.WHEN;
```

3개를 넘으면 `*` 한 줄로 바뀐다:

```java
import static laboratory.constants.Const.*;
```
