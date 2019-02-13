---
layout: post
date: 2015-10-20 22:47:00 +0900
title: 'AngularJS: 핸들러에서 이벤트가 발생한 노드에 접근하는 방법'
categories:
  - angularjs
tags:
  - javascript
  - angularjs
  - event
  - selector
---

* Kramdown table of contents
{:toc .toc}

#### window.event

```js
$scope.handler = function() {
    var currentObj = window.event.currentTarget;
};
```

#### $event

```html
<button ng-click="handler($event)">눌러주세요</button>
<script>
$scope.handler = function($event) {
    var currentObj = $event.target;
};
</script>
```

`window.event`와 `$event`는 같은 타입의 객체가 아니다.
