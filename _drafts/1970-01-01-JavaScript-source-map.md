---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'JavaScript: source map'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - sourcemap
---

#### 참고한 글
- [https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/](https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
- [https://blog.outsider.ne.kr/916](https://blog.outsider.ne.kr/916)

```js
//# sourceMappingURL=maps/swiper.min.js.map
```

min 파일과 원본 파일을 연결해주는 소스맵 파일이 있는데, 이 소스맵 파일에 대한 선언을 min 파일 내부에서 하는 방식임.

**jQuery 1.9.1 min파일의 sourcemap 정의**
```js
/*! jQuery v1.9.1 | (c) 2005, 2012 jQuery Foundation, Inc. | jquery.org/license
//@ sourceMappingURL=jquery.min.map
*/
(function(e,t){var n,r,i=typeof t,o=e.document,a=e.....
// 생략
```

소스맵 키워드였던 `//@`은 앞으로 사용하지 않으니 `//#`으로 [대체하라고 한다](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Errors/Deprecated_source_map_pragma?utm_source=mozilla&utm_medium=firefox-console-errors&utm_campaign=default).


V3 스펙에 따른 소스맵 파일 예시:
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
