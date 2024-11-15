---
layout: post
date: 2016-01-08 17:16:00 +0900
title: '[Java] Java: String의 인코딩과 케릭터 셋'
categories:
  - java
tags:
  - java
  - character-set
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [http://d2.naver.com/helloworld/19187](http://d2.naver.com/helloworld/19187)
- [http://d2.naver.com/helloworld/76650](http://d2.naver.com/helloworld/76650)
- [http://blog.javarouka.me/2011/09/new-string.html](http://blog.javarouka.me/2011/09/new-string.html)
- [유니코드 영역 \| 위키백과](https://ko.wikipedia.org/wiki/유니코드_영역)


## 개요

자바 String의 인코딩과 케릭터 셋에 대한 내용 정리. 

이 문서에서 인코딩 규칙은 '인코딩'으로, 문자와 숫자 간 변환 과정은 '부호화'와 '복호화'로 표기한다.


## 시스템 기본 인코딩 확인 Platform's Default Encoding

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


## 인코딩 찾기

인코딩 방식이 뭐였는지 **간단하게** 확인하는 방법은 사실상 없다. 

`String`의 생성자: 

```
String(byte[] bytes): 바이트 배열을 플랫폼의 기본 charset으로 복호화하여 새로운 String을 생성
String(byte[] bytes, Charset charset): 바이트 배열을 주어진 charset으로 복호화하여 새로운 String을 생성
```

다만, 생성자를 이용하여, 특정 문자의 바이트가 어떤 케릭터 셋의 코드값과 일치하는지 일일이 확인하는 방법이 있긴 하다:

```java
byte[] bs = str.getBytes();
byte[] bs2 = str.getBytes("utf-8");

// 복호화를 시도했을 때 글자가 깨지지 않고 온전한 것을 찾는다.
System.out.println(new String(bs)); // UTF-8. 시스템 기본 케릭터 셋으로 복호화됨
System.out.println(new String(bs2, "utf-8")); // UTF-8
System.out.println(new String(bs2, "euc-kr")); // �������ㅼ���
```

이 방법은 모든 사용 가능한 모든 문자를 대입해보지 않는한 정확한 케릭터 셋은 찾기 힘들다는 한계가 있다.


## 인코딩 변환하기

### String은 인코딩이 없다

자바에서 `String` 객체는 내부적으로 문자를 UTF-16으로 표현한다.

인터넷에서 String의 인코딩 변환 방법을 검색하면 이런 결과들이 나오곤 하는데:

```java
String originalText = "한글123";
byte[] bytes = originalText.getBytes(StandardCharsets.UTF_8);
String convertedText = new String(bytes, Charset.forName("EUC-KR"));

log.debug("convertedText: {}", convertedText); // 12 ab 媛���
```

**결과부터 말하면 엉터리다**. 얼핏 보기엔 UTF-8에서 EUC-KR 인코딩으로 변경하는 것처럼 보이지만 실상은 그렇지 않다.

`originalText`는 플랫폼이나 실제 입력된 값을 자바가 알아서 관리하는 String 값이다. 내부적으로는 UTF-16으로 다뤄지긴 하지만 개발자가 신경쓰지 않아도 된다.

`originalText.getBytes(StandardCharsets.UTF_8)`는 문자열을 UTF-8 바이트 배열로 부호화하고 그 배열을 반환한다. 

`new String(bytes, Charset.forName("EUC-KR"))`는 바이트 배열을 EUC-KR 인코딩으로 복호화하여 String 값으로 만든다. 

이 과정에서 UTF-8과 EUC-KR에서 동일한 규칙이 적용되는 ASCII 문자는 아무 문제가 없지만, 한글을 비롯한 유니코드 문자는 두 인코딩 방식 사이의 규칙이 다르므로 위의 예시처럼 `媛`, `�`, 혹은 그 유명한 `占쏙옙` 같은 깨진 문자열을 얻게 된다.

### 그럼 어떻게 해야 하는가?

앞서 말했듯 String 객체는 UTF-16으로 내부 데이터를 관리한다. `string.getBytes()` 같은 메서드가 실행되면 문자를 분석하여 유니코드 코드 포인트를 식별한 뒤, 지정된(혹은 시스템 기본 설정의) 인코딩 방식을 적용한다. 

이 과정은 개발자가 모르게 알아서 수행된다. 따라서 String 타입의 문자열은 인코딩이 없다고 봐도 무방하며, String 데이터 자체의 인코딩을 바꾸는 방법은 없다고 봐야 한다. 오직 이 값을 전송하거나 다른 데이터 타입으로 바꿀 때만 유효하다.

```java
final String text = "12 ab 가나";

byte[] bytes1 = text.getBytes(StandardCharsets.UTF_8);
byte[] bytes2 = text.getBytes(Charset.forName("EUC-KR"));

// UTF-8와 EUC-KR의 바이트 값 차이
log.debug("{}", bytes1); // [49, 50, 32, 97, 98, 32, -22, -80, -128, -21, -126, -104]
log.debug("{}", bytes2); // [49, 50, 32, 97, 98, 32, -80, -95, -77, -86]

// ASCII 문자인 숫자와 알파벳, 공백은 동일하지만 한글은 다르다
```

이렇게 바이트 배열로 변환한 시점이 인코딩이 적용된 상태니까, 이 값을 그대로 전달하거나 사용하는 게 올바른 방법이라 할 수 있다. 여기서 다시 String으로 바꾸는 것은 헛짓이다.
