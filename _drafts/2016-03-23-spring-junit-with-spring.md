---
layout: post
date: 2016-03-23 14:40:00 +0900
title: '[Spring] JUnit with Spring'
categories:
  - spring
tags:
  - java
  - spring
  - junit
  - code-test
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

-

```java
import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;

import org.apache.commons.codec.digest.DigestUtils;
import org.junit.Test;

public class JunitTest1 {
    @Test
    public void passwdDigest() {
        String originalPasswd = "1234tyu";
        String digested = DigestUtils.sha256Hex(originalPasswd);
        System.out.println(digested);
        String invalidpw = "123tyu";
        String validpw = "1234tyu";
        assertFalse(digested.equals(DigestUtils.sha256Hex(invalidpw)));
        assertTrue(digested.equals(DigestUtils.sha256Hex(validpw)));
    }
}
```

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import static org.junit.Assert.*;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/test-resource-context.xml")
public class JUnitTest2 {
    @Value("#{infoProp['shopinfo.s1.postNumber']}")
    private String shopProvinceName;

    @Test
    public void test(){
        assertEquals(shopProvinceName,"11111");
    }
}
```
