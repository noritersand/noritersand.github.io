---
title: 'eclipse: 다른 프로젝트를 포함하여 export 하는 방법'
categories:
  - tools
tags:
  - eclipse
  - deployment
  - todo
---

## 다른 프로젝트의 라이브러리를 참조하기
프로젝트가 다음처럼 있다고 가정한다.
- includeMe: 참조 대상
- primary: 주 작업 프로젝트

우선 참조 대상이 되는 includeMe 프로젝트의 `Properties` > `Java Build Path` > `Libraries`에서 `Add Jar` 후 `Order and Export`에서 추가된 라이브러리를 export 하도록 체크해야 한다. 이 작업을 하지 않으면 export 대상에서 제외되어 다른 프로젝트에서 라이브러리를 참조할 수 없다. 이후 primary 프로젝트의 `Properties` > `Java Build Path` > `Projects`에서 참조할 프로젝트인 includeMe를 `Add` 한다.

이 작업을 끝내면 primary 프로젝트에서는 includeMe 프로젝트의 jar를 import 할 수 있게된다.

## 참조하고 있는 다른 프로젝트를 같이 묶어서 war로 배포하기
지금까지의 작업은 이클립스 내에서, 즉 이클립스-톰캣 플러그인을 사용했을 때에만 가능한 작업이다.
프로젝트 `Properties` > `Deployment Assembly`에서 includeMe를 추가해도 WAR로 배포하고 나면 정상작동 하지 않는다. **확인할 것**
