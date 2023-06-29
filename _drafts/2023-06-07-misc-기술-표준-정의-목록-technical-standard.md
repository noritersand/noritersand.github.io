---
layout: post
date: 2023-06-07 11:06:52 +0900
title: '[misc] 기술 표준 정의 목록 technical standard'
categories:
  - misc
tags:
  - misc
  - standard
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)


## 개요

개발하면서 참고한 기술 표준들이 어느 문서 어디에 정의되어있는지 정리하는 문서.

**TODO 이 파일은 제목이 이상해서 수정해야 함.**


## 역주: 한 주의 시작은 월요일

한국에는 달력, 날짜 표기 등에 관한 일반적인 표준은 없지만, 정보를 교환할 때는 국제표준화기구 ISO에서 제정한 ISO 8601의 2004년 판을 그대로 채택(부합화 표준)하여 사용 중이라고 한다. [국립전파연구원 FAQ](https://www.rra.go.kr/ko/license/C_e_faq.do?fa_type=gl&fa_category=global)

- https://www.standard.go.kr/KSCI/standardIntro/getStandardSearchView.do?menuId=503&topMenuId=502&ksNo=KSXISO8601&tmprKsNo=KSXISO8601&reformNo=03
- 표준명: 데이터 요소 및 교환 포맷 ― 정보교환 ― 날짜 및 시각의 표기 (DATA ELEMENTS AND INTERCHANGE FORMATS -- INFORMATION INTERCHANGE -- REPRESENTATION OF DATES AND TIMES)
- 표준번호: KS X ISO 8601:2010
- 마지막 확인날짜: 2023-06-07

표준 원문의 2.2.8을 보면 다음처럼 정의돼있다:

> 역주(calendar week)
> 역년 중의 서수에 의하여 지정되는 특정한 7일의 기간으로 월요일부터 시작된다.

역주는 한 주를 부르는 말이다. 이 표준에선 일 년(역년)을 52-53주로 나누고, 한 주를 역주라 정의한다. 서수는 01(월요일)부터 시작해 07(일요일)로 끝난다.

### 처음 역주의 법칙: 역년의 첫 번째 역주는 일 년의 첫 번째 목요일이 포함된 주

2.2.10을 보면:

> 역주 수(calendar week number)
> 처음 역주의 법칙에 따르면, 역년 내, 역주를 나타내는 서수는 일 년의 첫 번째 목요일을 포함하는 수이다. 역년의 마지막 역주는 다음 역년의 첫 번째 역주 바로 이전의 주이다.

그리고 3.2.2를 보면:

> 1역년의 첫 번째 역주는 전 역년에서 3일까지 포함할 수 있으며, 1역년의 마지막 역주는 다음 해의 3일까지 포함하도록 한다. (생략)

라고 정의하는데, 이것은 일 년의 첫 번째 주에 대한 정의로, 역년의 첫 번째 목요일이 포함된 주를 첫 번째 역주로 정의한다는 말이다. 예를 들어 2023년 1월 1일은 일요일이라서 2022년 12월의 다섯 번째 주이며, 2023년의 첫 번째 주는 1월 2일 부터다.

### 역?

이 정의에서 말하는 역년, 역월, 역주, 역일에서 역은 曆이며 훈음은 '책력 력(역)'이다. 역법 혹은 달력에서의 역과 같다.
