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

#### 참고 문서

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
mapper.readValue(arg, new TypeReference<ArrayList<HashMap<String, String>>>() {});
```

ℹ️ 자바 최근(2025-03-16) 버전에선 `mapper.readValue(result, new TypeReference<>() {})` 라고만 해도 변수의 타입에 맞춰서 알아서 작동한다.

### map

맵 타입이 JSON 형식의 String 타입으로 변환된다. JavaScript에 JSON을 넘겨줄 때 유용하다.

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


## 어노테이션

### @JsonInclude

보통 아래처럼 `null`이 아닌 필드만 변환하도록 씀:

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
public class JsonResponse {
    // ... 생략
```

### @JsonIgnore

특정 필드를 JSON 변환 시(JSON 문자를 모델로 변환할 때도 포함이다) 무시하도록 하는 어노테이션. 메서드와 필드에 적용 가능하다.

```java
@Getter
@Setter
public class BaseYearMonthWeek {
    
    @JsonIgnore
    private String baseYearMonth;

}
```

### @JsonIgnoreProperties

JSON 변환 시 무시할 필드를 묶어서 지정할 수 있는 어노테이션. 

```java
@JsonIgnoreProperties({ "field1", "field2" })
public class MyClass {
    private String field1;
    private String field2;
    private String field3;
}
```

`allowGetters`나 `allowSetters` 속성을 통해 `getter()` 혹은 `setter()` 둘 중에 하나만 허용하도록 할 수 있다.

```java
@Getter
@Setter
@JsonIgnoreProperties(value = { "field1", "field2" }, allowSetters = true)
public class SomeApiRequest {
    private String field1; // 이 필드는 모델에서 JSON으로 변환할 때 무시됨
    private String field2; // 이 필드는 모델에서 JSON으로 변환할 때 무시됨
    private String field3;
}
```

### @JsonNaming

JSON 값의 네이밍 규칙과 자바 필드의 네이밍 규칙이 일치하지 않을 때, 클래스 레벨에 적용하는 어노테이션. 입력값과 출력값에 대한 형식을 지정하는 방식이다.

가령 입력값이 SNAKE_CASE일 때, 이 값을 자바 필드로 역직렬화(입력) 했다가 다시 직렬화(출력)할 때 SNAKE_CASE로 내보내고 싶다면 다음처럼 설정한다:

```java
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class Board {}
```

### @JsonProperty

`@JsonNaming`의 필드 레벨 버전으로, 각 필드마다 일일이 지정할 때 사용하는 어노테이션.

### @JsonAlias

앞의 `@JsonNaming`과 `@JsonProperty` 어노테이션은 직렬화와 역직렬화 모두 작동한다. 만약 역직렬화, 그러니까 입력값에 대해서만 형식을 지정하고 싶다면 이 어노테이션을 쓰면 된다.


## 문자열에서 java.time 패키지로 타입 변환하기

#### 테스트 환경 정보

- Java 17

"2021-03-91" 같은 문자열을 java.time 패키지의 `LocalDate`, `LocalDateTime` 같은 날짜 타입의 값으로 변환하려고 하면 이런 예외가 발생한다:

> com.fasterxml.jackson.databind.exc.InvalidDefinitionException: 
>   Java 8 date/time type `java.time.LocalDate` not supported by default: add Module "com.fasterxml.jackson.datatype:jackson-datatype-jsr310" to enable handling

에러 메시지가 알려주는 것처럼 [모듈](https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310)을 추가하면 해결된다.

### 해결 방법

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-jsr310 -->
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.14.2</version>
</dependency>
```

```java
@Getter
@Setter
public class ParseMe {
    private LocalDate birthDate;
}
```

```java
ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new JavaTimeModule());

String str = "{\"birthDate\":\"2018-01-01\"}";
ParseMe parseMe = mapper.readValue(str, ParseMe.class);
```
