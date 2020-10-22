---
layout: post
date: 2013-01-01 00:00:00 +0900
title: '[misc] markdown syntax'
categories:
  - misc
tags:
  - test
  - markdown
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax)
- [Markdown Guide: Bsic Syntax](https://www.markdownguide.org/basic-syntax)
- [kramdown: Quick Reference](https://kramdown.gettalong.org/quickref.html)
- [kramdown: Syntax](https://kramdown.gettalong.org/syntax.html)

## 줄 바꾸기 🤣

엔터 두 번은

`<p>` 태그로 분리하는 것과 같음.

스페이스바 2개 + 엔터 한 번은  
`<br>` 태그와 같음.

아니면 그냥 이렇게<br>
`<br>` 태그를 직접 작성해도 됨.

## 리스트

리스트:  
- 하나
- 둘
  - 둘둘
    - 둘하나하나
- 셋

#### 리스트:

- `code`
- `code`
- `code`

## 체크박스

- [x] 첫 번째 할 일
- [ ] 두 번째 할 일

## 헤더(이건 h2)

```
# h1
## h2
### h3
#### h4
##### h5
###### h6
```

### 헤더3 header3

#### 헤더4 header4

##### 헤더5 header5

###### 헤더6 이쯤 되면 본문보다 작을 수도 있다.

나보다 말야

## HTML 그대로 넣기

<ul>
  <li><a href="#항상-제목2부터-시작">항상 제목2부터 시작</a></li>
</ul>

## 항상 제목2부터 시작

내용내용

**굵게**
*이탤릭*
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

이렇게 ``` ` ``` 하면 됨. 이렇게 <code>\`</code> 하던가.

### 코드블록 안에 grave 표시하기 😏

```
`d`
```

<pre>
```  
야 너도 pre 태그 쓰면 할 수 이썽  
```
</pre>

### 키보드 입력 블록

키 입력을 표현할 땐 코드 블록 `ctrl + alt + shift + a`보다 이걸로 <kbd>ctrl + alt + shift + a</kbd>

## 테이블(표) 🙄

파이프`|`와 하이픈`-`으로 작성하며, 헤더 구분선의 콜론`:` 위치에 따라 좌/우/가운데 정렬함. 생략하면 좌측 정렬. 하이픈의 개수는 표의 모양과 상관없다.

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

백슬래시`\`를 사용해서 마크다운 문자를 이스케이프 하지 않고 그대로 표시  
\_ \`\`\` \*\* \#\# \-\-\- 뭐 이렇게...

## embed image

이건 그냥 기본 정렬  
~~![바쁜 라상무](/images/kakao-ryon-busy.png)~~

요거는 가운데 정렬  
![인라인 스타일 적용하는 방법](/images/all-k.jpg){: style="margin:0 auto; display:block;"}

## 첨자

마크업을 직접 사용함.

- `<sup>`: 위 첨자<sup>우효!</sup>
- `<sub>`: 아래 첨자<sub>우효옷!</sub>

## 주석: 각주 footnotes

요것[^1]은 각주 표기법이다. 마크다운이 아니고 kramdown에서 확장한 문법(일껄?).

## 주석: 첨자 활용

> 뿅뿅이라고(bbyong-bbyong)<sup>1</sup>라고 일컫는다<sup>2<sup>.

- 1: (역자주) 뿅뿅이란 말이다.
- 2: 출처: 뿅뿅 해석 완벽 가이드

## 루비 문자

\* 사실 루비 문자는 마크다운이 아니고 마크업이다.

<ruby><rb>으아아아</rb><rp>(</rp><rt>호옹이</rt><rp>)</rp></ruby>

- `<ruby>`: 루비 문자 영역 전체를 지정.
- `<rb>`: Ruby Bottom의 약자. 아래쪽에 들어갈 글자를 지정.
- `<rp>`: Ruby Parentheses의 약자. 브라우저가 루비를 지원하지 않을 경우 표시할 괄호를 지정.
- `<rt>`: Ruby Top의 약자. 위쪽에 들어갈 글자를 지정.

[^1]: 룰루랄라
