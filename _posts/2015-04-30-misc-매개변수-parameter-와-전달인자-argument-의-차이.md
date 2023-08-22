---
layout: post
date: 2015-04-30 16:19:00 +0900
title: '[misc] 매개변수(parameter)와 전달인자(argument)의 차이'
categories:
  - misc
tags:
  - misc
  - parameter
  - argument
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [위키백과: 매개변수](https://ko.wikipedia.org/wiki/%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98)
- [https://en.wikipedia.org/wiki/Parameter_%28computer_programming%29](https://en.wikipedia.org/wiki/Parameter_%28computer_programming%29)
- [https://msdn.microsoft.com/en-us/library/9kewt1b3.aspx](https://msdn.microsoft.com/en-us/library/9kewt1b3.aspx)
- [https://developer.mozilla.org/en-US/docs/Glossary/Parameter](https://developer.mozilla.org/en-US/docs/Glossary/Parameter)

> 종종 매개변수(parameter)와 전달인자(argument)는 적당히 섞어서 쓰이기도 하는데, 이 경우 문맥에 따라 의미를 달리해서 해석되기도 한다. 하지만 엄밀히 말해서 매개변수는 함수의 정의부분에 나열되어 있는 변수들을 의미하며, 전달인자는 함수를 호출할때 전달되는 실제 값을 의미한다. 이같은 의미를 명확히 하기 위해 매개변수는 변수(variable)로, 전달인자는 값(value)으로 보는 것이 일반적이다.

```java
int add(int a, int b) {
    // ...
}
```

a와 b를 매개변수(formal parameter)라고 하며,

```java
add(1, 2);
```

1과 2는 전달인자(actual argument)라고 한다.

전달인자는 줄여서 인자, 인수라고 부르기도 하는 모양.
