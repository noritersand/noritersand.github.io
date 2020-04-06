---
layout: post
date: 2016-12-19 13:51:00 +0900
title: '[Java] javax.validation'
categories:
  - java
tags:
  - java
  - javax
  - validation
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://docs.oracle.com/javaee/7/api/javax/validation/package-summary.html](https://docs.oracle.com/javaee/7/api/javax/validation/package-summary.html)
- [https://docs.oracle.com/javaee/7/api/javax/validation/ConstraintViolation.html](https://docs.oracle.com/javaee/7/api/javax/validation/ConstraintViolation.html)

```java
public class SomeException extends RuntimeException {
    private static final long serialVersionUID = 480486167019286584L;

    private Set<ConstraintViolation> violations;

    /**
     * @param errorCode
     * @param violations
     */
    public SomeException(String errorCode, Set<ConstraintViolation> violations) {
        super(errorCode);
        this.violations = violations;
    }

    /**
     * @return
     * @author fixalot
     */
    public Set<ConstraintViolation> getViolations() {
        return violations;
    }

    /**
     * @param violations
     * @author fixalot
     */
    public void setViolations(Set<ConstraintViolation> violations) {
        this.violations = violations;
    }
}
```

이건 뭐하는 소스인가?
