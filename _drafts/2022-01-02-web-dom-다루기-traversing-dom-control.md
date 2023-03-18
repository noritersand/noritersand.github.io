---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[WEB] DOM 다루기: Traversing'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - traversing
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)


## 개요

내 맴대로 분류한 DOM 탐색 관련 API 정리.

Element의 초기 선택은 Traversing이 아니라 Selector로 분류함.


## Element.prototype.children


## Element.prototype.previousElementSibling

선택한 요소의 바로 이전의 형제 요소를 가리키는 인스턴스 프로퍼티.

```js
document.querySelector('#first').previousElementSibling;
```

메서드가 아니라 프로퍼티이기 때문에 jQuery의 `.next(selector)` 처럼 특정 조건을 만족하는 요소를 찾으려면 요런게 있어야 함:

```js
function previousSibling(currentElement, condition) {
  let previousElement = currentElement
  while (true) {
    previousElement = previousElement.previousElementSibling;
    // 이전 형제 요소가 없어서 null이거나 주어진 조건식(condition)이 true면
    if (previousElement === null || condition(previousElement)) {
      break;
    }
  }
  return previousElement;
}
```

이전 형제라는 조건에 텍스트와 코멘트 노드도 포함이라면 이 프로퍼티 대신 `Node.prototype.previousSibling`을 써야 함.


## Element.prototype.nextElementSibling

선택한 요소의 바로 다음 형제 요소를 가리키는 인스턴스 프로퍼티.

```js
document.querySelector('#first').nextElementSibling;
```

이것도 역시 메서드가 아니라 프로퍼티이기 때문에 특정 조건으로 찾으려면 이렇게 해야 함:

```js
function nextSibling(currentElement, condition) {
  let nextElement = currentElement
  while (true) {
    nextElement = nextElement.nextElementSibling;
    // 다음 형제 요소가 없어서 null이거나 주어진 조건식(condition)이 true면
    if (nextElement === null || condition(nextElement)) {
      break;
    }
  }
  return nextElement;
}
nextSibling(document.querySelector('#root'), element => element.id === 'a11y-status-message');
```

탐색에 공백이나 텍스트 노드, 코멘트 노드를 포함하려면 `Element.prototype.nextElementSibling` 대신 `Node.prototype.nextSibling`을 사용한다.


## closest()

TODO
