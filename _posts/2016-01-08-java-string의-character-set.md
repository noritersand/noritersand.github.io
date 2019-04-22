---
layout: post
date: 2016-01-08 17:16:00 +0900
title: 'Java: Java: character set'
categories:
  - java
tags:
  - java
  - character set
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [http://d2.naver.com/helloworld/19187](http://d2.naver.com/helloworld/19187)
- [http://d2.naver.com/helloworld/76650](http://d2.naver.com/helloworld/76650)
- [http://blog.javarouka.me/2011/09/new-string.html](http://blog.javarouka.me/2011/09/new-string.html)
- [http://dev.epiloum.net/?s=%EC%9D%B8%EC%BD%94%EB%94%A9&submit=%EC%B0%BE%EA%B8%B0](http://dev.epiloum.net/?s=%EC%9D%B8%EC%BD%94%EB%94%A9&submit=%EC%B0%BE%EA%B8%B0)
- [https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_%EB%B8%94%EB%A1%9D](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_%EB%B8%94%EB%A1%9D)

## 시스템 기본 케릭터셋 확인 platform's default characterset

```java
System.getProperty("file.encoding");
```

## '한글'을 UTF-8로 인코딩 했을때의 16진수 표현값:

```java
String string = "한글";  
byte[] bytes = string.getBytes("UTF-8");  
for (byte b : bytes) {  
    System.out.print(String.format("0x%02X ", b));
}
```

## 인코딩 케릭터셋 찾기

인코딩 케릭터셋이 뭐였는지 **간단하게** 확인하는 방법은 사실상 없다. 특정 문자의 바이트가 어떤 케릭터셋의 코드값과 일치하는지 일일이 비교하는 방법이 있긴 하지만...

임시방편으로 아래와 같은 방법을 사용할 수 있는데:

```java
String(byte[] bytes): 바이트 배열을 플랫폼의 기본 케릭터셋으로 디코딩하여 새로운 String을 생성
String(byte[] bytes, Charset charset): 바이트 배열을 주어진 케릭터셋으로 디코딩하여 새로운 String을 생성
```

```java
byte[] bs = str.getBytes();
byte[] bs2 = str.getBytes("utf-8");

// 디코딩을 시도했을 때 글자가 깨지지 않고 온전한 것을 찾는다.
System.out.println(new String(bs)); // UTF-8. 시스템 기본 케릭터셋으로 디코딩 됨
System.out.println(new String(bs2, "utf-8")); // UTF-8
System.out.println(new String(bs2, "euc-kr")); // �������ㅼ���
```

이 방법은 모든 사용 가능한 모든 문자를 대입해보지 않는한 정확한 케릭터셋은 찾기 힘들다는 한계가 있다.
