---
layout: post
date: 2023-04-13 21:17:13 +0900
title: '[Dart] 다트 기본'
categories:
  - dart
tags:
  - dart
  - language
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://dart.dev/](https://dart.dev/)

#### 버전 정보

- x.x.x


## 개요

```c
void main() {
  print('Hello, World!');
}
```

구글에서 개발한 객체 지향 프로그래밍 언어. C의 영향을 받아 문법은 유사하지만 C 기반은 아니라고 한다.

참고로 던지며 노는 그 다트가 Dart다.


## 간단한 예시: SQLite DB에서 데이터 불러오기

```c
import 'package:sqflite/sqflite.dart';

void main() async {
  // 데이터베이스 열기
  var db = await openDatabase('my_db.db');

  // 쿼리 실행
  List<Map> result = await db.rawQuery('SELECT * FROM my_table');

  // 결과 출력
  result.forEach((row) => print(row));

  // 데이터베이스 닫기
  await db.close();
}
```
