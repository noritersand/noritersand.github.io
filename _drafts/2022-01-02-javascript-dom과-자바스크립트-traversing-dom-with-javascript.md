---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Traversing'
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

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 탐색 관련 API 정리.

최초 Element 선택은 Traversing이 아니라 Selector로 분류함.


## Element.children


## Element.nextElementSibling

선택한 요소의 바로 다음 형제 요소를 가리키는 `Element` 프로토타입의 인스턴스 프로퍼티.

```js
document.querySelector('#first').nextElementSibling;
```

메서드가 아니라 프로퍼티이기 때문에 jQuery의 `.next(selector)` 처럼 특정 조건을 만족하는 다음 형제를 찾으려면 반복문을 작성해야 한다:

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

탐색에 공백이나 텍스트 노드, 코멘트 노드를 포함하려면 `Element.nextElementSibling` 대신 `Node.nextSibling`을 사용한다.


## closest()


