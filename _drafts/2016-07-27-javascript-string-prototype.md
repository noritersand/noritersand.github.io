---
layout: post
date: 2016-07-27 12:01:00 +0900
title: '[JavaScript] String prototype'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - string
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)


## 개요

Standard built-in Objects: String

`String.prototype`의 자주 쓰이는 메서드와 프로퍼티 정리.


## 스태틱 프로퍼티

TODO


## 스태틱 메서드

TODO


## 인스턴스 프로퍼티

TODO


## 인스턴스 메서드

### String.prototype.replace()

```
replace(pattern, replacement)
```

`pattern`과 일치하는 문자열 찾아 `replacement`로 치환한다. 

- `pattern`: 검색 패턴. 문자열이거나 정규식일 수 있다. 단순 문자열인 경우 일치하는 첫 번째 문자열만 치환한다.
- `replacement`: 대체할 문자. 문자열이거나 함수일 수 있는데, 함수가 반환하는 값으로 치환한다.

```js
function replacer(match, offset, string) {
  console.log('match:', match);
  console.log('offset:', offset);
  console.log('string:', string);
  return 'b';
}

'qwe'.replace('q', replacer); // "bwe"
// match: q
// offset: 0
// string: qwe
```

### String.prototype.replaceAll()

```
replaceAll(pattern, replacement)
```

`replace()`와 거의 같지만, 검색된 모든 문자열을 치환한다는 점이 다르다.

### String.prototype.padStart()

TODO

### String.prototype.padEnd()

TODO
