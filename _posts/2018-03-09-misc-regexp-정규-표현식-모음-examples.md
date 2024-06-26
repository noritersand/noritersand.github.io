---
layout: post
date: 2018-03-09 17:53:21 +0900
title: '[misc] RegExp 정규 표현식 모음'
categories:
  - misc
tags:
  - misc
  - regex
  - regexp
  - regular-expression
  - rational-expression
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}


## RegExp 인스턴스 모음

### 빈 줄(empty/blank line)

```js
/^\n/
```

서브라임 텍스트에서 빈 줄 삭제할 때 씀. (regex 활성화하고 `^\n` 입력)

```js
/^\s*$/
```

### 줄의 첫 번째부터 시작하는 특정 문자

```js
/^##\s/
```

마크다운 문서에서 h2 태그 찾을 때 씀. 라인의 끝은 어찌돼도 상관 없으므로 `$`는 안붙여도 됨.

### 특정 문자열 중에 하나와 완전히 일치하는지 검사

```js
// 'lax' 혹은 'strict'인지
/^lax$|^strict$/
```

### 문자의 시작과 끝으로 범위 검색

```js
// 'a'와 'b'로 시작하며 줄 바꿈을 제외한 모든 문자를 포함한 문자
/a.*b/
// 'a'와 'b'로 시작하며 줄 바꿈을 제외한 모든 문자를 하나 이상 포함한 문자
/a.+b/
```

```js
// 'ja'로 시작하고 `pt`로 끝나는 문자 중 밑줄, 알파벳, 숫자가 하나 이상 포함된 문자
/ja\w+pt/
// 위랑 비슷하지만 'japt'처럼 중간 문자가 없어도 됨.
/ja\w*pt/
```

**TODO** 위에서 공백을 제외하는 방법을 찾아야됨.

### 특정 문자와 문자 사이의 넓은(?) 검색

```js
/name[-_]{0,}card/gi
```

설명:

- `namecard`: `name`에 이어지는 `card`를 검색
- `[-_]`: `name`과 `card`사이에 `-` 혹은 `_`가 있는지
- `{0,}`: `-` 혹은 `_`가 0개 이상 있는지
- `gi`: 전역/대소문자 무시

검색 가능한 문자 예시:

```js
nameCard NameCard name-card name--card NAME_CARD NAME___CARD
```

### 영문 대소문자, 숫자, 언더바`_`, 하이픈`-`, 2-5자 길이의 정규 표현식

```js
/^[a-zA-Z0-9_-]{2,5}$/
```

### 언더바`_` 이후 모든 문자

```js
/_\w+/
```

### 언더바`_` 이전 모든 문자

```js
/\w+_/
```

### 날짜 포맷: yyyy-MM-dd

```js
/[12][0-9]{3}-[0-9]{2}-[0-9]{2}/
/[12][0-9]{3}-[01][0-9]-[0-3][0-9]/
```

### 이미지 태그 찾기

#### &lt;img부터 &gt; 까지

```js
/<img [^>]*src="([^"]+)"[^>]*>/
```

#### &lt;img부터 `src="~~~"` 까지

```js
/<img [^>]*src="([^"]+)"/
```

#### 이미지 확장자가 jpg, png

```js
/<img [^>]*src="([^"]+)(([^"]+)(.)(jpg|png))"/
```

### 영문 대소문자, 숫자, 5-50자 길이

```js
/^[a-zA-Z0-9]{5,50}$/
```

### 첫 번째 `-`를 제외한 `-` 찾기

```js
/(?!^)-/g
```

OR 연산자`|`까지 섞어주면 이렇게 됨

```js
// 시작 위치의 -를 제외한 모든 -를 제거하고 숫자와 -를 제외한 모든 문자 제거
'-12 a s d 3-- a s d-'.replace(/(?!^)-|[^0-9\-]/g, ''); // -123
```

### 특정 문자로 시작하는 것은 제외

```js
// 앞에 단어 구성 문자가 있는 'Params'를 찾되, 앞의 문자가 'Search'인 것은 제외
var regex = /(?<!Search)\BParams\b/;
var testStrings = ['MemberParams', 'MemberSearchParams', 'Params', 'SearchParams'];
for (let str of testStrings) {
  console.log(`'${str}' matches: ${regex.test(str)}`);
}
// 'MemberParams' matches: true
// 'MemberSearchParams' matches: false
// 'Params' matches: false
// 'SearchParams' matches: false
```


## 함수 적용 예시 모음

### 숫자를 통화로

```js
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

### 통화를 숫자로

```js
'1,000,000'.replace(/[^0-9]/g, '');
```

### 통화를 숫자로 \#2

```js
'123,123,123'.replace(/\,/g, '');
```

### 빈 줄 제거(앞)

```js
var a = "\n\na\n\n";
a.replace(/^\s*[\r\n]/gm, "");
```

### 영문과 숫자가 아니면 뿅으로 치환

```js
'q!1@2#3뀨?asd한글'.replace(/[^A-Za-z0-9]/gi, '뿅');
// 'q뿅1뿅2뿅3뿅뿅asd뿅뿅'
```

### 숫자와 하이픈`-`이 아니면 지움

```js
'a1c2D박3뿅-뿅45_$'.replace(/[^0-9-]/g, ''); // '123-45'
```

### 숫자가 아닌 문자는 빈문자열로 치환

```js
'12-a-123-0qweoi-123'.replace(/\D/g, ''); // "121230123"
```

### 아스키 코드는 1, 유니코드는 3으로 치환

```js
string.replace(/[\0-\x7f]|([0-\u07ff]|(.))/g,"$&$1$2").length
```

### 내용의 값의 빈공백을 trim(앞/뒤)

```js
String.prototype.trim = function () {
  var TRIM_PATTERN = /(^\s*)|(\s*$)/g;
  return this.replace(TRIM_PATTERN, "");
};
```

### 이메일 형식 검사

```js
var reg = /[a-zA-Z0-9]+(?:(\.|_)[A-Za-z0-9!#$%&'*+/=?^`{|}~-]+)*@(?!([a-zA-Z0-9]*\.[a-zA-Z0-9]*\.[a-zA-Z0-9]*\.))(?:[A-Za-z0-9](?:[a-zA-Z0-9-]*[A-Za-z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?/g;
var value = 'abc@qwe.org';
console.assert(reg.test(value), '이메일 형식이 아님');
```

```js
function isValidEmail(data) {
  var format = /^((\w|[\-\.])+)@((\w|[\-\.])+)\.([A-Za-z]+)$/;
  if (data.search(format) != -1)
    return true; //올바른 포맷 형식
  return false;
}
```

[이곳](http://emailregex.com/) 참고.

### 한글 필터링

```js
function isValidKorean(data) {
   // 유니코드 중 AC00부터 D7A3 값인지 검사
  var format = /^[\uac00-\ud7a3]*$/g;
  if (data.search(format) == -1)
    return false;
  return true; //올바른 포맷 형식
}
```

### 한글 필터링 2

```js
function checkKoreanWord(str) {
  var check = /[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/;
  return check.test(str) ? true : false;
}
```

### 이미지 파일인지 검사

```js
function isValidNumber(data) {
  var format=/^[0-9]*$/g;
  if(data.search(format) == -1)
    return false;
  return true;
}
```

### 숫자인지 검사

```js
function isValidNumber(data) {
  var format=/^[0-9]*$/g;
  if(data.search(format) == -1)
    return false;
  return true;
}
```

```js
'a12345'.match(/[^0-9]/) != null // 숫자가 아닌 문자가 있을 경우 true
```

### yyyy-MM-dd 날짜 입력 제한

```js
format = /[12][0-9]{3}-[01][0-9]-[0-3][0-9]/; //YYYY-MM-DD 검사표현식
//if(f.birth.value.search(format)==-1)
if (! format.test(f.birth.value)) {
  alert("생년월일을 정확하게 입력하세요");
  f.birth.focus();
  return;
}
```

### 숫자만 입력하게 함

```js
if (! /^(\d+)$/.test(f.age.value)) {
  alert("숫자만 입력하세요.");
  f.age.focus();
  return;
}
```

### 영문, 숫자, 특수문자( . ; - )인지 검사

```js
var text = $('#inptMemId').val();
var regexp = /[0-9a-zA-Z.;\-]/; // 숫자,영문,특수문자
// var regexp = /[0-9]/; // 숫자만
// var regexp = /[a-zA-Z]/; // 영문만
for (var loop = 0; loop < text.length; loop++) {
  if (text.charAt(loop) != " "
      && regexp.test(text.charAt(loop)) == false) {
    alert(text.charAt(loop) + "는 입력불가능한 문자입니다");
    return;
    break;
  }
}
```

### 숫자와 알파벳 대소문자, 쉼표`.`, 언더바`_` 이외의 문자가 있을 경우 false

```js
/^[a-zA-Z0-9._]*$/.test('123abcABC_.');
```

### 특수문자가 있으면 true

```js
function existSpecialchar(str) {
  var pattern = /[`~!@#$%^&*|\\\'\";:\/?]/gi;
  if (pattern.test(str) == true) {
    return true;
  }
  return false;
}
```

### 숫자만 있으면 true

```js
function isOnlyNumber(str) {
  var pattern = /^(\d+)$/gi;
  return pattern.test(str);
}
```

### 한글과 공백만 있으면 true

```js
function isOnlyKoreanWithSpaces(str) {
  var pattern = /^[가-힣\s]+$/gi;
  return pattern.test(str);
}
```

### 숫자와 하이픈(-)만 true, 자릿수 체크 안함

```js
/^((\d+)(\-?)(\d+))+$/.test('010-0000-0000')
```

### HTML에서 특정 속성 검색

```js
var p = /placeholder=[\"']?([^>\"']+)[\"']?[^>]/;
p.test('placeholder="abc"'); // true
p.test('placeholder=""'); // false
```

`placeholder="abc"`는 찾지만 `placeholder=""`는 못찾음.
