---
layout: post
date: 2013-07-25 21:30:00 +0900
title: '[Java] java.util.Properties'
categories:
  - java
tags:
  - java
  - properties
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://docs.oracle.com/javase/10/docs/api/java/util/Properties.html](http://docs.oracle.com/javase/10/docs/api/java/util/Properties.html)

자바 Properties 클래스의 간단한 사용방법을 기술한 글.

## java.util.Properties

```java
Properties prop = new Properties();
prop.put("서울의수도", "서울에수도가어디있어");
System.out.println(prop); // {서울의수도=서울에수도가어디있어}

Map<String, String> map = new HashMap<>();
map.put("한국의수도", "서울");
System.out.println(map); // {한국의수도=서울}
```

사용방법은 해시맵과 거의 차이가 없지만 파일 입출력 기능을 지원하며 저장할 수 있는 값의 타입은 String으로만 지정할 수 있다. 보통 Properties 클래스는 자바 애플리케이션에서 사용될 상수를 텍스트 파일로 미리 작성한 뒤 필요한 곳에서 그 파일을 읽어오는데 사용된다.

프로퍼티 파일은 대충 아래와 같은 모양이다:

![](/images/properties-1.png)

## 주요 메서드

### store()

객체에 저장된 데이터를 파일로 출력한다.

```
store( OutputStream out, String comments )
store( Writer writer, String comments )
```

```java
FileOutputStream fos = new FileOutputStream("C:/dev/temp/test.properties"); // 상대경로를 지정하면 루트는 '워크스페이스/프로젝트'
Properties prop = new Properties();
prop.put("서울의수도", "서울에수도가어디있어");
prop.store(fos, "코멘트테스트");
fos.close();
```

![](/images/properties-2.png)

### load()

맵 형태로 저장된 파일의 내용으로 Properties 인스턴스를 생성한다.

```
load( InputStream inStream )
load( Reader reader )
```

```java
FileInputStream fis = new FileInputStream("C:/dev/temp/test.properties");
Properties prop = new Properties();
prop.load(fis);
fis.close();

prop.list(System.out); // 서울의수도=서울에수도가어디있어
logger.debug(prop.getProperty("서울의수도")); // 서울에수도가어디있어
```

### loadFromXML()

XML 형식의 파일을 읽어서 Properties 인스턴스를 생성한다.

```
loadFromXML( InputStream in )
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>URL information</comment>
    <entry key="image.root">http://tistory.com</entry>
    <entry key="some.korean">한글</entry>
</properties>
```

```java
@Test
public void testPropFromXml() throws IOException {
    FileInputStream fis = new FileInputStream("src\\test\\resources\\properties\\url.xml");
    Properties prop = new Properties();
    prop.loadFromXML(fis);
    fis.close();
    Assert.assertEquals("{some.korean=한글, image.root=http://tistory.com}",
            prop.toString());

    Enumeration<Object> keys = prop.keys();
    while (keys.hasMoreElements()) {
        String key = (String) keys.nextElement();
        System.out.println("key: " + key + ", value: " + prop.getProperty(key));
    }

    // key: some.korean, value: 한글
    // key: image.root, value: http://tistory.com
}
```

### keySet()

Properties는 HashMap과 마찬가지로 서수가 없기 때문에 프로퍼티만큼 반복하려면 Iterator나 Enumeration이 필요한데, 이때 사용되는 메서드가 `keySet()`이다.

```
Set<K> keySet(): Returns a Set view of the keys contained in this map.
```

```java
Properties prop = new Properties();
prop.setProperty("서울", "매우많음");
prop.setProperty("대전", "조금많음");
prop.setProperty("인천", "많음");

FileOutputStream fos = new FileOutputStream("test.properties");
prop.store(fos, "시도별 인구 수");
fos.close();

FileInputStream fis = new FileInputStream("test.properties");
prop.load(fis);
fis.close();

logger.debug(prop.values());

-> [많음, 매우많음, 조금많음]

Iterator<Object> it = prop.keySet().iterator();

while(it.hasNext()) {
    String key = (String) it.next();
    String value = (String) prop.getProperty(key);
    logger.debug(key + " : " + value);
}

-> 인천 : 많음
-> 서울 : 매우많음
-> 대전 : 조금많음
```

끗.
