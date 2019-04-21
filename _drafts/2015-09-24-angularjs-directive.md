---
layout: post
date: 2015-09-24 14:41:00 +0900
title: 'AngularJS: directive'
categories:
  - angularjs
tags:
  - javascript
  - angularjs
  - directive
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- 어디어디

```js
myModule.directive('myMain', ['$window', function($window){
    return {
        templateUrl:'/layer/my_main.html',
        controller:'myMainCtrl',
        replace:true,
        link:function($scope, el, attr) {
            $scope.loadMyFunction(el, attr);
        }
    };
}]);
```
