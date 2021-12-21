---
layout: post
date: 2021-12-14 18:40:49 +0900
title: '[misc] ELK Stack 메모'
categories:
  - misc
tags:
  - elasticsearch
  - logstash
  - kibana
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [ELK Stack: Elasticsearch, Logstash, Kibana | Elastic](https://www.elastic.co/kr/what-is/elk-stack)

ELF 스택의 목적은 로그 분석이다.

원래는 Elasticsearch, Logstash, Kibana를 조합하여 구성하는 걸 말하는데 여기에 Filebeat를 섞기도 한다. 이럴 경우 흐름은:

```
Filebeat -> Logstash -> Elasticsearch -> Kibana
```

## [Filebeat](https://www.elastic.co/kr/beats/filebeat)

로그 수집기. 얘 없이 ELK 스택 구성 가능. elastic사의 Beats 하위 앱임. 단독 앱이며 원하는 로그 파일을 읽어서 어디론가 전송하는 기능만 있는 경량 앱.

## [Logstash](https://www.elastic.co/kr/logstash/)

역시 단독 앱이며 특정 파일을 직접 혹은 통신을 통해 읽어들인 로그 파일을 지정된 포맷으로 변환한다. 변환한 파일을 어디론가 전송하면 이 뇨솤이 하는 일은 끝. ELK 스택에선 Elasticsearch 쪽으로 전송하게 됨.

## [Elasticsearch](https://www.elastic.co/kr/elasticsearch/)

(공식 사이트에 따르면) 분산형 RESTful 검색 및 분석 엔진. ELS 스택에서 Logstash가 전달한 로그들의 분석/검색을 담당함.

## [Kibana](https://www.elastic.co/kr/kibana/)

Elasticsearch의 데이터를 시각화하는 GUI 툴.
