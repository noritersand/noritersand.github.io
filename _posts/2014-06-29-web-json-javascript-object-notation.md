---
layout: post
date: 2014-06-29 16:32:00 +0900
title: '[Web] JSON, JavaScript Object Notation'
categories:
  - web
tags:
  - json
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Introducing JSON](https://www.json.org/json-en.html)
- [JSON - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [RFC 8259 - The JavaScript Object Notation (JSON) Data Interchange Format](https://datatracker.ietf.org/doc/html/rfc8259)
- [ECMA-404 - Ecma International](https://ecma-international.org/publications-and-standards/standards/ecma-404/)


## 개요

데이터를 표현하는 방법 중 하나. 뜻을 직역하면 자바스크립트 객체 표기법이다. 경량이며 읽고 쓰기 편리하다는 특징이 있다. 이름과는 달리 특정 프로그래밍 언어에 의존하지 않는다. 그래서 XML이나 CSV와 같은 범용 데이터 교환 포맷으로 사용된다.

**자바스크립트의 객체 리터럴 표기법(Object literal notation)과는 문법 차이가 있다.** 둘의 가장 큰 차이점은 다음과 같다:

- JSON은 함수를 정의할 수 없다.
- JSON에서 이름과 값(문자열에만 해당)을 감싸는 큰따옴표
는 생략하거나 작은따옴표
로 대체할 수 없다.

```
{ "이름": 값 }
```

JSON은 기본적으로 이름과 값을 쌍(*Key-Value Pair*)으로 갖는 형태를 취한다. 값은 문자열(string), 숫자(number), `true`, `false`, `null`, 배열(array)과 또다른 JSON 객체(object)만이 허용된다. 이름과 값의 한 쌍을 프로퍼티라고 할 때, 하나 이상의 프로퍼티는 쉼표로 구분한다.

```json
{
  "prop1": "따옴표로 둘러 싸인 UNICODE 문자들의 조합",
  "prop2": 123456789.1,
  "prop3": false,
  "prop4": null,
  "prop5": [1, 2, 3],
  "prop6": {
    "alphabet": "abcdefg"
  }
}
```

최상위 객체는 배열로 대체할 수 있다. JSON array의 표기법은 자바스크립트와 같다. 대괄호`[]`로 감싸진 쉼표로 구분된 값의 나열이다.

```json
[1, 2, 3, 4]
```

```json
[
  {"firstObject": 1},
  {
    "secondObject": {
      "prop1": 1234,
      "prop2": "abcd"
    }
  }
]
```


## JavaScript와 JSON

### JSON.parse()

```
JSON.parse( jsString )
```

- `jsString`: 객체로 변환할 문자열


자바스크립트는 JSON을 다루기 위한 네이티브 메서드를 제공한다. 이 중 `JSON.parse()`는 JSON 문자열을 자바스크립트 객체로 변환하여 반환하는데, 이 때 전달된 인자가 JSON 규격에 맞는지 엄격히 체크한다.

```js
JSON.parse( "{num: 123}" );  // SyntaxError: JSON.parse: expected property name or '}'
JSON.parse( "{'num': 123}" );  // SyntaxError: JSON.parse: expected property name or '}'
JSON.parse( '{"num": 123}' );  // Object { num: 123 }
JSON.parse( '{"fn": function () {}}' ); // SyntaxError: JSON.parse: unexpected keyword at line 1 column 9 of the JSON data
```

jQuery는 `$.parseJSON()` 메서드를 제공한다. 만약 브라우저가 `JSON.parse()`를 지원하지 않는다면 `$.parseJSON()`은 전달된 문자열을 그대로 반환한다.

### JSON.stringify()

```
JSON.stringify( jsObject )
```

- `jsObject`: 문자열로 변환할 자바스크립트 객체

`JSON.stringify()`는 자바스크립트 객체를 JSON 문자열로 변환하는 메서드다.

```js
var foo = {
  num: 123,
  name: "who i'm"
};

JSON.stringify(foo); // "{"num":123,"name":"who i'm"}"
```

⚠️ 이 메서드는 값이 `undefined`인 경우 변수나 프로퍼티를 없는 것처럼 취급하니 주의:

```js
var a = null;
JSON.stringify(a); // "null"

var b = undefined;
JSON.stringify(b); // undefined

var o = {
  a: null,
  b: undefined
};
JSON.stringify(o); // '{"a":null}' 
```


## JAVA와 JSON

```
[{ "이름1":"값1", "이름2":"값2"}, {"이름1":"값1", "이름2":"값2" }]
```

위의 JSON을 JAVA의 String 리터럴로 만들려면 아래처럼 되겠지만:

```java
n++;
sb.append("[");
sb.append("{");
sb.append("\"num\":\"" + n + "\"");
sb.append(", \"name\":\"" + name + n + "\"");
sb.append(", \"content\":\"" + content + n + "\"");
sb.append("}");

n++;
sb.append(", {");
sb.append("\"num\":\"" + n + "\"");
sb.append(", \"name\":\"" + name + n + "\"");
sb.append(", \"content\":\"" + content + n + "\"");
sb.append("}");
sb.append("]");

out.print(sb);
```

이렇게 일일이 작성하는 것은 힘들고 귀찮은 일이다. 그래서 보통은 `org.json.JSONObject`와 같은 서드 파티를 이용한다:

```java
// 단일 객체
JSONObject ob = new JSONObject();
ob.put("이름1", "값1");
ob.put("이름2", "값2");

logger.debug(ob.toString()); // {"이름2":"값2","이름1":"값1"}

// 객체 배열
JSONArray list = new JSONArray();

JSONObject ob2 = new JSONObject();
ob2.put("이름1", "값1");
ob2.put("이름2", "값2");
list.put(ob2);

ob2 = new JSONObject();
ob2.put("이름1", "값1");
ob2.put("이름2", "값2");
list.put(ob2);

logger.debug(list.toString()); // [{"이름2":"값2","이름1":"값1"},{"이름2":"값2","이름1":"값1"}]
```
