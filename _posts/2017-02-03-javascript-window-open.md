---
layout: post
date: 2017-02-03 13:38:10 +0900
title: '[JavaScript] Window.open()'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - window.open
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.w3schools.com/jsref/met_win_open.asp](http://www.w3schools.com/jsref/met_win_open.asp)
- [MDN: Window.open()](https://developer.mozilla.org/ko/docs/Web/API/Window/open)

## open()

```
window.open( URL, name [ , specs ] [ , replace ] )
```

- `URL`: 문서의 URL
- `name`: URL이 로드될 창의 이름을 지정하거나 기존의 창 이름을 지정할 경우 해당 윈도우를 재사용한다.
- `specs`: 창의 위치와 크기, 기능 등 창이 갖는 특성을 지정한다. 이 값은 브라우저/버전마다 적용이 되기도 하고 무시되기도 한다. 가령 `location`은 과거엔(IE 구버전에선 아직도 적용됨) 숨길 수 있었으나, 최근엔 대부분의 브라우저에서 보안상의 이유로 무시하는 값이다.
- `replace`: true일 땐 새 문서가 이전의 문서와 교체된다. false이거나 지정되지 않으면 새 문서는 창의 브라우징 히스토리에 새 항목으로 추가된다.

```js
function openNewWindow(url, name) {
  var specs = "left=10,top=10,width=372,height=466";
  specs += ",menubar=no,toolbar=yes,location=no,status=yes,titlebar=no,scrollbars=yes,resizable=yes";
  window.open(url, name, specs);
}
```

**name**: 연결된 문서를 읽어 지정한 이름의 윈도우나 프레임(frame)을 target으로 설정한다. 지정된 이름을 가진 윈도우가 없으면 새 창으로 처리된다.

- `_blank`: 연결된 문서를 읽어 새로운 빈 윈도우에 표시한다. 윈도우 이름은 없다.
- `_media`: 연결된 문서를 읽어 미디어바의 HTML 내용부분에 표시한다. IE6부터 적용된다.
- `_parent`: 연결된 문서를 읽어 바로 상위 모체창에 표시한다.
- `_search`: 연결된 문서를 읽어 브라우저의 검색창에 표시한다. IE5부터 적용된다.
- `_self`: 디폴트이며, 연결된 문서를 읽어 현재창에 표시한다.
- `_top`: 연결된 문서를 읽어 최상위 윈도우에 표시한다.

URL과 name만 명시하면 대부분의 브라우저에서 새 탭으로 처리된다. 만약 단순 새 탭이나 새 창이 아닌 '팝업'이라 통용되는 창을 생성하려 한다면 사이즈(width, height)를 명시해야 한다:

```js
window.open("/test.jsp", "팝업", "left=10, top=10, width=500, height=500");
```

화면 정중앙에 새 창을 띄우려면 다음처럼 부모창의 window 객체가 리턴하는 사이즈 값을 이용해 처리한다.

```js
var width=800, height=500;
var left = (screen.availWidth - width)/2;
var top = (screen.availHeight - height)/2;
var specs = "width=" + width;
specs += ",height=" + height;
specs += ",left=" + left;
specs += ",top=" + top;

window.open("/test.jsp", "팝업", specs);
```

## 브라우저별 특성

### 크롬

새 창 옵션 중 menubar가 yes일 경우 크롬은 새 창 팝업 대신 새 탭 팝업으로 열린다.

### 파이어폭스


### 엣지
