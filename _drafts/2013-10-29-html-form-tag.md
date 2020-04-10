---
layout: post
date: 2013-10-29 14:59:00 +0900
title: '[HTML] form tag'
categories:
  - html
tags:
  - html
  - form
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/HTML/Element/form](https://developer.mozilla.org/ko/docs/Web/HTML/Element/form)
- [https://www.w3schools.com/tags/tag_form.asp](https://www.w3schools.com/tags/tag_form.asp)

## form

todo

## fieldset

```html
<fieldset>
    <legend>제목</legend>
    내용
</fieldset>
```

form field set 태그라고 하며 웹페이지의 내용을 그룹화 하는데 사용된다. form 태그와 같이 사용할 경우에, fieldset 태그에 disabled 속성을 주면 하위 폼 컨트롤들이 모두 비활성화 된다.

```html
<form method="post" action="#">
    <fieldset>
        <legend>주문 상세 페이지</legend>
        <table cellspacing="0" cellpadding="0">
            <caption>주문 상세 페이지</caption>
            <colgroup>
                <col width="15%"/>
                <col width="85%"/>
            </colgroup>
            <tbody>
                <tr>
                <!-- 이하 생략 -->
```

## label

```html
<form action="">
    <fieldset>
        <legend>하이</legend>
        <input type="checkbox" id="a1"/><label for="a1" title="풍선도움말">남자</label>
        <input type="checkbox" id="a2"/><label for="a2">여자</label>
    </fieldset>
</form>
```

체크박스, 라디오 버튼 등과 함께 사용하는 태그로 폼 컨트롤을 직접 선택하지 않고 텍스트만으로도 폼 요소를 선택할 수 있게 해준다. title 속성에 값이 있을경우 마우스 오버 시 툴팁이 나타난다.
