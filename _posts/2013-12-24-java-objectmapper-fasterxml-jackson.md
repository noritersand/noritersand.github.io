---
layout: post
date: 2013-12-24 10:15:00 +0900
title: '[Java] ObjectMapper (FasterXML/jackson)'
categories:
  - java
tags:
  - java
  - fasterxml
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [http://fasterxml.com](http://fasterxml.com)
- [https://github.com/FasterXML](https://github.com/FasterXML)

**jackson은 1.x 버전과 2.x 버전의 패키지가 다르다.**
- 1.x: org.codehaus.jackson
- 2.x: com.fasterxml.jackson

## Map/Collections - JSON간 변환

### writeValueAsString()

```
writeValueAsString( value )
```

- `value`: String 타입으로 변환할 대상

### readValue()

```
readValue( arg, type )
```

- `arg`: 지정된 타입으로 변환할 대상
- `type`: 대상을 어떤 타입으로 변환할것인지 클래스를 명시한다. Class, TypeReference 타입이 올 수 있다.

```java
mapper.readValue(arg, ArrayList.class);
mapper.readValue(arg, new ArrayList<HashMap<String, String>>().getClass());
mapper.readValue(arg, new TypeReference<ArrayList<HashMap<String, String>>>(){});
```

### map

맵 타입이 JSON 형식의 String 타입으로 변환된다. 자바스크립트에 JSON을 넘겨줄 때 유용하다.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Test2 {
    public static void main(String[] args) throws Exception {

        ObjectMapper mapper = new ObjectMapper();

        HashMap<String, String> map = new HashMap<String, String>();
        map.put("name", "steave");
        map.put("age", "32");
        map.put("job", "baker");

        System.out.println(map);
        System.out.println(mapper.writeValueAsString(map));
    }
}

// {age=32, name=steave, job=baker}
// {"age":"32","name":"steave","job":"baker"}
```

위와 반대로 JSON을 맵 타입으로 변환하려면 다음처럼 작성한다:

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;

public class Test2 {
    public static void main(String[] args) throws Exception {

        ObjectMapper mapper = new ObjectMapper();
        HashMap<String, String> map = new HashMap<String, String>();

        String jsn = "{\"age\":\"32\",\"name\":\"steave\",\"job\":\"baker\"}";

        map = mapper.readValue(jsn,
                new TypeReference<HashMap<String, String>>() {});        

        System.out.println(map);
    }
}

// {name=steave, age=32, job=baker}
```

### Collections<Map>

다음은 view에 전달할 model이 `List<map<?, ?>>` 타입일 때 이를 JSON으로 변환하는 방법이다. 사용방법은 크게 다르지 않고 `writeValueAsString()`, `readValue()` 메서드를 사용하되 타입 명시만 달리한다.

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;

public class Test2 {
    public static void main(String[] args) throws Exception {

        ObjectMapper mapper = new ObjectMapper();

        // map -> json
        ArrayList<HashMap<String, String>> list
                = new ArrayList<HashMap<String,String>>();

        HashMap<String, String> map = new HashMap<String, String>();
        map.put("name", "steave");
        map.put("age", "32");
        map.put("job", "baker");
        list.add(map);

        map = new HashMap<String, String>();
        map.put("name", "matt");
        map.put("age", "25");
        map.put("job", "soldier");
        list.add(map);

        System.out.println(mapper.writeValueAsString(list));

        // json -> map
        String jsn = "[{\"age\":\"32\",\"name\":\"steave\",\"job\":\"baker\"},"
                + "{\"age\":\"25\",\"name\":\"matt\",\"job\":\"soldier\"}]";
        list = mapper.readValue(jsn,
                new TypeReference<ArrayList<HashMap<String, String>>>() {});        

        System.out.println(list);
    }
}

// [{"age":"32","name":"steave","job":"baker"},{"age":"25","name":"matt","job":"soldier"}]
// [{name=steave, age=32, job=baker}, {name=matt, age=25, job=soldier}]
```
