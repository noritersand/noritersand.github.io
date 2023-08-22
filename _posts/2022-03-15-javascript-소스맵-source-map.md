---
layout: post
date: 2022-03-15 16:20:40 +0900
title: '[JavaScript] 소스맵 Source Map'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - sourcemap
---

#### 관련 문서

- [Introduction to JavaScript Source Maps](https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
- [\[MDN\] Use a source map](https://developer.mozilla.org/en-US/docs/Tools/Debugger/How_to/Use_a_source_map)
- [자바스크립트와 커피스크립트에서 소스맵(source map) 사용하기](https://blog.outsider.ne.kr/916)


## 개요

```js
//# sourceMappingURL=my-awesome-script.min.js.map
```

소스맵은 배포용으로 빌드된(combined/minified/uglyfied), 그러니까 보통 'min'이란 이름이 붙는 자바스크립트 파일의 원본을 연결해서 클라이언트 실행 환경에 영향을 끼치지 않으면서 원본 소스를 읽거나 디버깅할 수 있게 하는 기술이다.

제대로 연결했다면 브라우저에서 선언부를 확인하려고 할 때 min 파일 대신 브라우저가 생성한 원본을 보여준다:

![](/images/sourcemap-example.png)


## 선언

소스맵 키워드였던 `//@`은 앞으로 사용하지 않으니 `//#`으로 [대체하라고 한다](https://developer.mozilla.org/en-US/docs/Tools/Debugger/How_to/Use_a_source_map).

다음은 V3 스펙을 따르는 예시다:

```js
{
    version : 3,
    file: "out.js",
    sourceRoot : "",
    sources: ["foo.js", "bar.js"],
    names: ["src", "maps", "are", "fun"],
    mappings: "AAgBC,SAAQ,CAAEA"
}
```


## jQuery의 sourcemap

jQuery 1.9.1을 보면 소스맵 선언을 이렇게:

```js
/*! jQuery v1.9.1 | (c) 2005, 2012 jQuery Foundation, Inc. | jquery.org/license
//@ sourceMappingURL=jquery.min.map
*/
(function(e,t){var n,r,i=typeof t,o=e.document,a=e... }) // 생략
```

그리고 `jquery.min.map`은 이렇게 생겼다:

```js
{
  "version":3,
  "file":"jquery.min.js",
  "sources":["jquery.js"],
  "names":["window",
  "undefined",
  "readyList",
  "rootjQuery",
  "core_strundefined",
  "document",
  "location",
  "_jQuery",
  "jQuery",
  "_$",
  "$",
  "class2type",
  "core_deletedIds",
  "core_version",
  // 이하 생략
}
```

끝.
