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

- [https://daringfireball.net/projects/markdown/syntax](https://daringfireball.net/projects/markdown/syntax)
- [https://www.markdownguide.org/](https://www.markdownguide.org/)

**결정 못한 작성 규칙**

- 나열할 리스트의 제목을 본문 서식 vs 제목4 서식

리스트:

- 첫
- 두
- 세

#### 리스트:

- `code`
- `code`
- `code`

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

**굵게**
_이탤릭_
~~취소선~~

## 수평선

---

[글 내부 링크 걸기](#항상-제목2부터-시작)

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

### 인라인 코드블록 안에 grave(=backtick) 표시하기

이렇게 ``` ` ``` 하면 됨.

### 코드블록 안에 grave 표시하기

```
`d`
```

## 테이블

파이프`|`와 하이픈`-`으로 작성하며, 헤더 구분선의 콜론`:` 위치에 따라 좌/우/가운데 정렬함. 생략하면 좌측 정렬.

|                  | String      | Number | Boolean | Object                 |
|:-----------------|-------------|:------:|---------|-----------------------:|
| undefined        | "undefined" | NaN    | false   | TypeError              |
| true             | "true"      | 1      |         | new Boolean(true)      |
| "1.2"            |             | 1.2    | true    | new String("1.2")      |
| pipe             | p\|i\|p\|e  |        |         |                        |

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
>>>>>>> WX-78
>>>>>>>> 팔푼이
>>>>>>>>> 비둘기야먹자 구구 구구궄

## unescape

- `\#`: '#'를 그대로

## 루비 문자

<ruby><rb>으아아아</rb><rp>(</rp><rt>호옹이</rt><rp>)</rp></ruby>

- `<ruby>`: 루비 문자 영역 전체를 지정.
- `<rb>`: Ruby Bottom의 약자. 아래쪽에 들어갈 글자를 지정.
- `<rp>`: Ruby Parentheses의 약자. 브라우저가 루비를 지원하지 않을 경우 표시할 괄호를 지정.
- `<rt>`: Ruby Top의 약자. 위쪽에 들어갈 글자를 지정.

## 주석: 첨자 활용

> 뿅뿅이라고(closure)<sup>1</sup>라고 일컫는다<sup>2<sup>.

- 1: 뿅뿅이다.
- 2: (역자주) 뿅뿅이란 말이다.
- 출처: 뿅뿅 해석 완벽 가이드

## 각주 footnotes

요것[^1]은 각주 표기법이다. 기본 마크다운인지는 모르겠음.

- [^1]: 룰루랄라
