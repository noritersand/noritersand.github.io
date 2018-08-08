---
title: 'JavaScript: source map'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - sourcemap
---

#### 참고한 글
- https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/
- https://blog.outsider.ne.kr/916

```js
//# sourceMappingURL=maps/swiper.min.js.map
```

min 파일과 원본 파일을 연결해주는 소스맵 파일이 있는데, 이 소스맵 파일에 대한 선언을 min 파일 내부에서 하는 방식임.

min 파일의 소스맵 선언 키워드였던 `//@`은 앞으로 사용하지 않으니 `//#`로 [대체하라고 한다](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Errors/Deprecated_source_map_pragma?utm_source=mozilla&utm_medium=firefox-console-errors&utm_campaign=default).


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
