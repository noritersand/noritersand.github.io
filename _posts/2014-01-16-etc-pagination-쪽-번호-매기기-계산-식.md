---
layout: post
date: 2014-01-16 14:00:00 +0900
title: 'etc: Pagination 쪽 번호 매기기 계산 식'
categories:
  - java
  - jsp
tags:
  - java
  - jsp
  - paging
  - pagination
---

* Kramdown table of contents
{:toc .toc}

[쪽수계산식](/attachments/calculate-for-pagination.xlsx)

쪽 번호 매기기 계산(페이지 인덱싱) 식에 관한 정리

#### currentPage

현재 페이지

#### rowsPerPage

한 페이지 당 출력할 게시물. 고정값으로 처리하거나 사용자가 선택한 값을 받는다.

#### totalRows

total number of rows, 모든 데이터의 개수. `SELECT COUNT(*)`의 값을 의미한다.

#### pageLength(혹은 totalPage, maxPage)

총 페이지 수를 의미하며 아래처럼 계산한다.

```java
pageLength = totalRows / rowsPerPage + ((totalRows % rowsPerPage == 0) ? 0 : 1)
```

이때 `totalRows / rowsPerPage`의 소숫점 이하는 버린다.

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
