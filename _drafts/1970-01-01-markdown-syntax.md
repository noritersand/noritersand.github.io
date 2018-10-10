---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'markdown syntax'
categories:
  - html
  - markdown
tags:
  - test
---

#### 참고한 글
- [어디어디](/assad)

## embed image
~~![바쁜 라상무](/images/image-kakao-ryon-busy.png)~~

<h4>목차</h4><ul><li><a href="#항상-제목2부터-시작">항상 제목2부터 시작</a></li><ul><li><a href="#코드-블럭">코드 블럭</a></li><ul><li><a href="#제목4">제목4</a></li></ul></ul></ul>

## 항상 제목2부터 시작
내용내용

# 큰제목
## 둘째
### 셋째
#### 제목4

리스트:
- 첫
- 두
- 세

**굵게**
_이탤릭_
~~스트라이크~~

---


[링크 걸기](#항상-제목2부터-시작)

## 코드 블럭

```
  코드 블럭
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
코드블럭 안에 backticks(=graves) 표시하기 `d`
```

## 테이블

|                  | String      | Number | Boolean | Object                 |
|------------------|-------------|--------|---------|------------------------|
| undefined        | "undefined" | NaN    | false   | TypeError              |
| true             | "true"      | 1      |         | new Boolean(true)      |
| "1.2"            |             | 1.2    | true    | new String("1.2")      |
