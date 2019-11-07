---
layout: post
date: 2013-07-24 17:38:00 +0900
title: '[Java] java.lang.String'
categories:
  - java
tags:
  - java
  - string
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://docs.oracle.com/javase/10/docs/api/java/lang/String.html](http://docs.oracle.com/javase/10/docs/api/java/lang/String.html)
- [http://d2.naver.com/helloworld/76650](http://d2.naver.com/helloworld/76650)

## escape sequence

| Escape sequence | Meaning                                                                                                     |
|-----------------|-------------------------------------------------------------------------------------------------------------|
| `\b`              | Backspace                                                                                                   |
| `\f`              | Form feed (rarely used)                                                                                     |
| `\n`              | Line feed (newline)                                                                                         |
| `\r`              | Carriage return. Use with the line feed `\r\n` to format output.                                            |
| `\t`              | Horizontal tab                                                                                              |
| `\v`              | Vertical tab. Not compliant with ECMAScript standard and incompatible with Microsoft Internet Explorer 6.0. |
| `\'`              | Single quote `'`                                                                                            |
| `\"`              | Double quote `"`                                                                                            |
| `\\`              | Backslash `\`                                                                                               |
| `\n`              | ASCII character represented by the octal number n. The value of n must be in the range 0 to 377 (octal).    |
| `\xhh`            | ASCII character represented by the two-digit hexadecimal number hh.                                         |
| `\uhhhh`          | Unicode character represented by the four-digit hexadecimal number hhhh.                                    |

## 주요 메서드

### intern()

String 인스턴스의 문자열을 "constant pool (상수 풀)"에 등록한다.  만약 constant pool 에 이미 존재하는 문자열이라면 그 문자열의 참조값을 리턴한다.

[누군가](http://stackoverflow.com/questions/1091045/is-it-good-practice-to-use-java-lang-string-intern)가 말하길, 동등비교가 가능한 것은 부수효과일 뿐 `intern()`의 원래 목적은 메모리 관리라고 한다.

```java
public static void main(String[] args) {
    String a = "AAA";
    String b = new String("AAA");
    System.out.println(a == b); // false

    b = b.intern();
    System.out.println(a == b); // true
}
```

### codePointAt()

String의 각 문자에 해당하는 유니코드 코드 포인트를 10진수로 리턴한다.

```java
String str = "한글";
for (int i = 0; i < str.length(); i++) {
    System.out.print(String.format("U+%04X ", str.codePointAt(i)));
}
```
