---
layout: post
date: 2017-02-03 13:44:00 +0900
title: '[JavaScript] Array'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - array
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm](http://www.bennadel.com/blog/1796-javascript-array-methods-unshift-shift-push-and-pop.htm)
- [http://programmingsummaries.tistory.com/357](http://programmingsummaries.tistory.com/357)

#### Array.prototype.join()

#### Array.prototype.concat()


## 스택기능

### Array.prototype.push()

배열의 맨 뒤에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.push('a'); // 1
arr.push('b', 'c', 'd'); // 4
console.log(arr); // ['a', 'b', 'c', 'd']
```

### Array.prototype.pop()

TODO

### Array.prototype.unshift()

`push()`와 반대로 배열의 맨 앞에 요소를 더하고 배열의 길이를 반환한다.

```js
var arr = [];
arr.unshift('a'); // 1
arr.unshift('b', 'c', 'd'); // 4
console.log(arr); // ['b', 'c', 'd', 'a']
```

### Array.prototype.shift()

## 탐색

### Array.prototype.forEach()

```
forEach( callback [, thisArg ] )
```

여기서 `callback`은:
```
forEach( element, index, object )
```

TODO: 자바에서 `forEach()`류(정확히는 stream이지만)에선 continue/break 사용이 불가능했는데, js도 그러는지 확인 필요함.

### Array.prototype.map()

### Array.prototype.some()

### Array.prototype.every()

### Array.prototype.filter()

```js
let arr = [1, 2, 3, 4];
let even = arr.filter(function(element) {
  return element % 2 == 0;
});
even; // [2, 4]
```

### Array.prototype.reduce()

## ~~주작~~조작

### Array.prototype.slice()

### Array.prototype.splice()
