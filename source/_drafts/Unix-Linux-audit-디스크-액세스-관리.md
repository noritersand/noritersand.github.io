---
title: 'Unix/Linux: audit 디스크 액세스 관리'
categories:
  - os
  - unix/linux
tags:
  - unix
  - linux
  - audit
---

#### 참고한 글
- https://linux.die.net/man/8/auditctl
- https://www.lesstif.com/pages/viewpage.action?pageId=20776444
- [http://amerika.tistory.com/entry/audit설정을-통합-리눅스-파일-보안강화](http://amerika.tistory.com/entry/audit%EC%84%A4%EC%A0%95%EC%9D%84-%ED%86%B5%ED%95%9C-%EB%A6%AC%EB%88%85%EC%8A%A4-%ED%8C%8C%EC%9D%BC-%EB%B3%B4%EC%95%88%EA%B0%95%ED%99%94)
- https://techintguru.blogspot.kr/2017/04/audit.html


여러가지 할 수 있는걸로 보이는데, 일단 아는건 파일 시스템의 변화를 감지하고 로그를 남길 수 있음.
