---
layout: post
date: 2015-11-10 12:49:00 +0900
title: '[misc] MS Office 엑셀 트릭 모음'
categories:
  - misc
tags:
  - misc
  - excel
  - spreadsheet
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [마소 공식 도움말: Excel의 바로 가기 키](https://support.microsoft.com/ko-kr/office/excel%EC%9D%98-%EB%B0%94%EB%A1%9C-%EA%B0%80%EA%B8%B0-%ED%82%A4-1798d9d5-842a-42b8-9c99-9b7213f0040f)

## 단축키

- <kbd>f4</kbd>: `$` 표시(절대 셀 지정 방식) 토글. 셀 선택 후 누르면 뭔가 하는데 뭔지 몲
- <kbd>f5</kbd> <kbd>ctrl + g</kbd>: 셀 이동/선택 기능. 단순 셀 이동(마지막 수정 셀로 이동도 됨) 기능과 옵션<kbd>alt + s</kbd> 선택으로 원하는 셀을 동시에 선택하는 기능이 있다.
- <kbd>alt + q</kbd>: 빠른 찾기
- <kbd>ctrl + e</kbd>: 자동 채우기
- 셀 수정모드에서 <kbd>ctrl + enter</kbd>: 선택한 셀에 입력한 값 동시 입력

## 연산자와 문법

### 문자열 연산

```js
// A2 셀에 'Tail' 붙이기
=A2 & "Tail"
```

## 함수

## 서식

### 시간 관련

#### 날짜 데이터를 계산(뺄셈 등)해서 나온 실수를 시간으로 환산

|  |  |`=B2 - B1` (`셀 서식 > 표시 형식 > 사용자 지정 > 형식 > [h]"시간" mm"분"`)|
|--|--|--|
|2015-11-06 11:22|2015-11-09 9:21|2.915926 (114시간 11분)|

#### 시간값을 1시간 기준의 실수값으로 환산

|  |=TEXT(B1, "ddhhmm")|`=LEFT(B2, 2) * 24 + MID(B2, 3, 2) + RIGHT(B2, 2) / 60`|
|--|--|--|
|12:40|001240|12.67|
