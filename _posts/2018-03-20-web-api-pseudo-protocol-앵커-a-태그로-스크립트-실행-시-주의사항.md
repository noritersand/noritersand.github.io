---
layout: post
date: 2018-03-20 17:00:42 +0900
title: '[Web API] pseudo protocol 앵커(a) 태그로 스크립트 실행 시 주의사항'
categories:
  - web-api
tags:
  - web-api
  - javascript
  - ecmascript
  - anchor
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [void operator \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void)

#### 테스트 환경

- explorer 10
- chrome 43
- firefox 38
- opera 30


## 개요

문서의 어느 한 요소를 클릭했을 때 지정한 함수를 호출하도록 HTML을 작성한다고 하자. 이럴 때 흔히들 `<a>` 태그의 href 속성을 이용하여 스크립트를 실행하곤 한다.

```html
<a href="javascript:getSome()"> 눌러요 </a>
```

'javascript: pseudo protocol' 혹은 그냥 'javascript protocol', 'javascript prefix'라고 하는 이 방식은 사실 `<a>` 태그의 매우 잘못된 사용법이다. 대신 `<button>` 태그와 onclick 속성의 조합이 권장되는데, 불가피하게 `<a>` 태그를 꼭 사용해야 한다면 최소한 아래 내용은 숙지하는게 좋다.

```html
<!DOCTYPE html>
<html>
<head>
<script>
  function fn() {
    console.log('hi');
    return true;
  }
</script>
</head>
<body>
  <a href="javascript:fn()">pushme</a>
</body>
</html>
```

위 코드를 브라우저로 열어서 `<a>` 태그를 클릭하면 무슨 일이 일어날까?

![첨부이미지1](/images/javascript-pseudo-protocol-1.png)

파이어폭스와 IE는 앵커에 의해 스크립트가 실행될 때, 해당 스크립트가 반환하는 undefined 이외의 값을 URL로 간주하는 경향이 있다. 위 스샷은 이 때문에 발생하는 현상이다. 에러라면 에러라고 할 수 있는 현상인데, 이를 방지하기 위해선 아래 방법 중 하나를 따른다. (크롬과 오페라는 해당 없음. 사파리는 패스)


## 함수가 undefined를 반환

자바스크립트의 함수는 브레이스`}`  혹은 return을 만났을 때 undefined를 반환한다. 즉, 반환값을 명시하지 않는 방법이다.

```html
<script>
  function fn() {
    console.log('hi');
  }
</script>

<a href="javascript:fn()">pushme</a>
```


## onclick="return false"

반환값을 정의할 수 없는 함수를 호출할 때 사용한다. (가령 거의 대부분 객체를 반환하는 jQuery의 메서드들) 이러한 경우에 onclick 이벤트 핸들러가 false를 반환하게 하여 브라우저의 기본 기능을 막아버리는 방법이다. (이벤트 핸들러가 false를 반환하면 태그의 기본 기능과 이벤트 전파(버블링, 캡처링)를 차단하는 효과가 있다.)

```html
<script>
  function fn() {
    console.log('hi');
    return 'arrrrgh';
  }
</script>

<a href="javascript:fn()" onclick="return false">pushme</a>
```


## void 연산자 사용

void 연산자가 피연산자를 실행하되 피연산자의 값을 무시하고 무조건 undefined를 반환하는 특성을 이용한 방법이다.

```html
<a href="javascript:void window.open()">open</a>
```
