---
layout: post
date: 2013-07-24 23:30:00 +0900
title: '[Java] java.util.Iterator'
categories:
  - java
tags:
  - java
  - iterator
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [http://docs.oracle.com/javase/10/docs/api/java/util/Iterator.html](http://docs.oracle.com/javase/10/docs/api/java/util/Iterator.html)

## java.util.Iterator

이터레이터는 자바 컬렉션 프레임워크(Java Collections Framework)에서 Enumeration을 대체하는 인터페이스로 객체들의 집합을 구성하는 각각의 요소를 순차적으로 추출할 때 사용된다. Enumeration과 비교해 메서드명이 개선되었으며 호출측에서 집합의 요소를 삭제할 수 있게 한다.

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove();
}
```

## 주요 메서드

- `hasNext()`: Iterator 객체가 다음요소를 소유하는지 여부를 불리언으로 반환.
- `next()`: 다음요소를 반환.
- `remove()`: 컬렉션에서 Iterator 의해 반환된 마지막 요소를 제거.

## example

```java
import java.util.HashMap;
import java.util.Iterator;

public class LogicTest {
    public static void main(String[] args) throws Exception {

        HashMap<String, Integer> map = new HashMap<>();
        map.put("일공일", 101);
        map.put("이공이", 202);
        map.put("삼공삼", 303);

        Iterator<String> it = map.keySet().iterator();

        while(it.hasNext()) {
            String key = it.next();
            int value = map.get(key);
            logger.debug(key + " : " + value);
        }
    }
}
```

## 컬렉션과 이터레이터

컬렉션 중 하나인 List는 인덱스가 존재하여 Iterator 없이도 향상된 반복문을 사용할 수 있다:

```java
ArrayList<String> list = new ArrayList<String>();
list.add("하나");
list.add("둘");
list.add("셋");

for (String ele : list) {
    logger.debug(ele);
}

for (int loop=0; loop<list.size(); loop++) {
    logger.debug(list.get(loop));
}

HashSet<Integer> set = new HashSet<>();
set.add(1);
set.add(3);
set.add(2);

for (Integer ele : set) {
    logger.debug(ele);
}


하나
둘
셋
하나
둘
셋
1
2
3
```

Map 타입에서도 순서대로 값을 가져오고 싶다면 어떻게 해야할까. 맵은 약간 다른 방식으로 접근해야한다.

List와 Set에서 Iterator를 사용하려면:

```java
ArrayList<String> list = new ArrayList<String>();
list.add("하나");
list.add("둘");
list.add("셋");

Iterator<String> it = list.iterator();

while(it.hasNext()) { // Iterator 객체에 다음 데이터가 있으면 true
    String ele = it.next(); // 다음 데이터를 반환
    logger.debug(ele);
}

HashSet<Integer> set = new HashSet<>();
set.add(1);
set.add(3);
set.add(2);

Iterator<Integer> it2 = set.iterator();

while(it2.hasNext()) {
    int ele = it2.next();
    logger.debug(ele);
}
```

반면 Map에선:

```java
ArrayList<String> list = new ArrayList<String>();
HashMap<String, Integer> map = new HashMap<>();
map.put("일공일", 101);
map.put("이공이", 202);
map.put("삼공삼", 303);
```

여기서 `keySet()` 메서드로 key값을 `Set`으로 추출한다:

```java
Set<String> set = map.keySet();
logger.debug(set);
```

`Set`은 Iterator가 있기 때문에 반복문으로 value를 가져올 수 있다:

```java
Iterator<String> it = set.iterator();

while(it.hasNext()) {
    String key = it.next();
    int value = map.get(key);
    logger.debug(key + " : " + value);
}
```

번거롭다면 향상된 반복문을 써도 된다:

```java
Iterator<String> it = set.iterator();

for(String key : set) {
    int value = map.get(key);
    logger.debug(key + " : " + value);
}
```
