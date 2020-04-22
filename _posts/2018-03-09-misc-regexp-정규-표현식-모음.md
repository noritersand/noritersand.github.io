---
layout: post
date: 2018-03-09 17:53:21 +0900
title: '[misc] RegExp 정규 표현식 모음'
categories:
  - misc
tags:
  - regex
  - regexp
  - regular-expression
  - rational-expression
  - code-snippet
---

## misc

### 공백 줄(empty line)

```js
/^\n/
```

```js
/^\s*$/
```

### 문자의 시작과 끝으로 범위 검색

```js
// 'a'와 'b'로 시작하며 줄바꿈을 제외한 모든 문자를 포함한 문자
/a.*b/
// 'a'와 'b'로 시작하며 줄바꿈을 제외한 모든 문자를 하나 이상 포함한 문자
/a.+b/
```

```js
// 'ja'로 시작하고 `pt`로 끝나는 문자 중 밑줄, 알파벳, 숫자가 하나 이상 포함된 문자
/ja\w+pt/
// 위랑 비슷하지만 'japt'처럼 중간 문자가 없어도 됨.
/ja\w*pt/
```

TODO 위에서 공백을 제외하는 방법을 찾아야됨.

### 영문 대소문자, 숫자, 언더바`_`, 하이픈`-`, 2~5자 길이의 정규 표현식

```js
/^[a-zA-Z0-9_-]{2,5}$/
```

## 함수 사용 예시

### 숫자 - 통화 치환

```js
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

### 통화 - 숫자 치환 \#1

```js
'1,000,000'.replace(/[^0-9]/g, '');
```

### 통화 - 숫자 치환 \#2

```js
'123,123,123'.replace(/\,/g, '');
```

### 빈줄 제거(앞)

```js
var a = "\n\na\n\n";
a.replace(/^\s*[\r\n]/gm, "");
```

### 영문과 숫자가 아니면 뿅으로 치환

```js
'q!1@2#3뀨?asd한글'.replace(/[^A-Za-z0-9]/gi, '뿅');
// 'q뿅1뿅2뿅3뿅뿅asd뿅뿅'
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
String.prototype.trim = function() {
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
function isValidEmail(data){
  var format = /^((\w|[\-\.])+)@((\w|[\-\.])+)\.([A-Za-z]+)$/;
  if (data.search(format) != -1)
    return true; //올바른 포맷 형식
  return false;
}
```

[이곳](http://emailregex.com/) 참고.

### 한글 필터링

```js
function isValidKorean(data){
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
  return check.test(str)?true:false;
}
```

### 이미지 파일인지 검사

```js
function isValidNumber(data){
  var format=/^[0-9]*$/g;
  if(data.search(format) == -1)
    return false;
  return true;
}
```

### 숫자인지 검사

```js
function isValidNumber(data){
  var format=/^[0-9]*$/g;
  if(data.search(format) == -1)
    return false;
  return true;
}
```

```js
'a12345'.match(/[^0-9]/) != null // 숫자가 아닌 문자가 있을 경우 true
```

### 생년월일

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

### 날짜 형식 검사 정규 표현식(yyyy-MM-dd)

```js
function isValidDateFormat(data) {
  var format = /[12][0-9]{3}-[0-9]{2}-[0-9]{2}/;
  if(data.search(format) == -1)
    return false;

  var _year = data.substr(0,4);
  var _month= data.substr(5,2);
  var _day = data.substr(8,2);

  return isValidDate(_year, _month, _day);
}
```

### 영문, 숫자, 특수문자( . ; - )인지 검사

```js
var text = $('#inptMemId').val();
var regexp = /[0-9a-zA-Z.;\-]/; // 숫자,영문,특수문자
// var regexp = /[0-9]/; // 숫자만
// var regexp = /[a-zA-Z]/; // 영문만
for (var loop = 0; loop < text.length; loop++){
  if (text.charAt(loop) != " "
      && regexp.test(text.charAt(loop)) == false) {
    alert(text.charAt(loop) + "는 입력불가능한 문자입니다");
    return;
    break;
  }
}
```

### 영문 대소문자, 숫자, 5~50자 길이의 정규 표현식

```js
/^[a-zA-Z0-9]{5,50}$/
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

### 언더바`_` 이후 모든 문자

```js
/_\w+/
```

### 언더바`_` 이전 모든 문자

```js
/\w+_/
```

## 이미지 태그 찾기

### `<img`부터 `>` 까지

```js
/<img [^>]*src="([^"]+)"[^>]*>/
```

### `<img`부터 `src="~~~"` 까지

```js
/<img [^>]*src="([^"]+)"/
```

#### 이미지 확장자가 `jpg`, `png`

```js
/<img [^>]*src="([^"]+)(([^"]+)(.)(jpg|png))"/
```
