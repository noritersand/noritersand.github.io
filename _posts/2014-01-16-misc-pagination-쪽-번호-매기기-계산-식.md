---
layout: post
date: 2014-01-16 14:00:00 +0900
title: '[misc] Pagination 쪽 번호 매기기 계산 식'
categories:
  - misc
tags:
  - misc
  - paging
  - pagination
---

* Kramdown table of contents
{:toc .toc}

## 개요

쪽 번호 매기기 계산(페이지 인덱싱)식에 관한 정리글.

[쪽수계산식이 적용된 엑셀 표 다운로드 링크](/attachments/calculate-for-pagination.xlsx)

#### currentPage

현재 페이지

#### rowsPerPage

한 페이지 당 출력할 게시물. 고정값으로 처리하거나 사용자가 선택한 값을 받는다.

#### totalRows

total number of rows, 모든 데이터의 개수. `SELECT COUNT(*)`의 값을 의미한다.

#### pageLength(혹은 totalPage, maxPage)

총 페이지 수를 의미하며 아래처럼 계산한다:

```js
pageLength = totalRows / rowsPerPage + ((totalRows % rowsPerPage == 0) ? 0 : 1)
```

이 때 `pageLength`의 소숫점 이하는 버리거나, 아니면 처음부터 소수점 올림 함수를 이용한다:

```js
// 자바스크립트일 때, Math.ceil()은 매개변수를 소수점 올림하여 반환하는 함수.
pageLength = Math.ceil(totalRows / rowsPerPage)
```

#### indexesPerPage

Number of indexes per page, 한 페이지에서 표시 가능한 최대 인덱스 수. pageLength가 이 값보다 큰 경우 페이지 자체도 인덱싱이 가능해야 한다.

그림으로 설명하면 indexesPerPage가 5이고 pageLength가 6일 때

![](/images/page-index-1.png)

위처럼 indexesPerPage를 초과하는 페이지는 한번에 표시하지 않으며 버튼 클릭 시 나타나도록 구현한다.

![](/images/page-index-2.png)

#### startNumber, endNumber

DB검색 시 사용될 한 페이지의 시작 값과 끝 값(게시물 번호를 의미함). currentPage와 rowsPerPage를 이용해 계산한다:

```
startNumber = (currentPage == 1) ? 1 : (currentPage - 1) * rowsPerPage + 1
endNumber = currentPage * rowsPerPage;
```
