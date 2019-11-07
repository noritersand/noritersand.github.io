---
layout: post
date: 2018-01-01 00:00:00 +0900
title: '[demo] markdown syntax'
categories:
  - demo
tags:
  - test
  - markdown
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](/somewhere)

## header2

### header3

#### header4

## embed image

~~![바쁜 라상무](/images/kakao-ryon-busy.png)~~

## HTML 그대로 넣기

<ul>
  <li><a href="#항상-제목2부터-시작">항상 제목2부터 시작</a></li>
</ul>

## 항상 제목2부터 시작

내용내용

리스트:
- 첫
- 두
- 세

**굵게**
_이탤릭_
~~취소선~~

## 수평선

---

[링크 걸기](#항상-제목2부터-시작)

## 코드 블록

```
  코드 블록
```

```java
  public class Test {
    public static void main(String... args) {
      System.out.println("야이야이야"); // dasfasdf
    }
  }
```

`inline codeblock`

```
코드블록 안에 backticks(=graves) 표시하기 `d`
```

## 테이블

|                  | String      | Number | Boolean | Object                 |
|------------------|-------------|--------|---------|------------------------|
| undefined        | "undefined" | NaN    | false   | TypeError              |
| true             | "true"      | 1      |         | new Boolean(true)      |
| "1.2"            |             | 1.2    | true    | new String("1.2")      |
| pipe            |  p\|i\|p\|e  |        |         |                        |

## 인용구

> 블라블라는 블라블라다
>
> 출처: 깐따삐야

> 1단 인용구
>> 2단 인용구
>>> 3단 인용구
>>>> 어디까지 가나 보자
>>>>> 독수리오형제
>>>>>> 씩쓰투스
>>>>>>> WX-칠
>>>>>>>> 팔이
>>>>>>>>> 비둘기야먹자

## 인용구2

> 뿅뿅이라고(closure)<sup>1</sup>라고 일컫는다<sup>2<sup>.

- 1: 뿅뿅이다.
- 2: (역자주) 뿅뿅이란 말이다.
- 출처: 뿅뿅 해석 완벽 가이드

## unescape

- `\#`: '#'를 그대로
