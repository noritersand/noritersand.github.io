---
layout: post
date: 2016-07-06 16:55:10 +0900
title: '[JavaScript] 정규 표현식, Regular Expressions(regexp) in JavaScript'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - regex
  - regexp
  - standard-built-in-objects
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://www.regexr.com/](http://www.regexr.com/)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [http://blog.outsider.ne.kr/360](http://blog.outsider.ne.kr/360)
- [http://regexlib.com/DisplayPatterns.aspx](http://regexlib.com/DisplayPatterns.aspx)
- [http://blog.eairship.kr/197](http://blog.eairship.kr/197)
- [http://tryhelloworld.co.kr/courses/정규표현식](http://tryhelloworld.co.kr/courses/정규표현식)


정규 표현식이란 문자열에서 특정한 캐릭터 조합을 찾아내기 위한 패턴이다. 줄여서 정규식이라고도 한다.

## 초기화

### object initializers

```
/pattern/[flags]
```

```js
/MSIE/.test(window.navigator.userAgent);
```

### constructor function

```
new RegExp( pattern [, flags ] )
```

```js
var re = new RegExp("ab+c", "i");
```

## flag

- `g`: 전역 검색. 지정된 범위 내에서 모두 검색하라는 의미다. 이 플래그가 없으면 메서드에 따라 첫 번째 검색 결과만 반환함
- `i`: 대/소문자 무시
- `gi`: 대/소문자 무시하고 전역 검색
- `m`: 멀티 라인 검색
- `y`: Perform a "sticky" search that matches starting at the current position in the target string.

## 정규 표현식과 함께 사용하는 함수

- `String.search( regexp )`: 정규식 패턴에 첫 번째로 일치하는 부분 문자열의 위치를 반환 하며 존재하지 않으면 `-1`을 반환한다.
- `String.match( regexp )`: 지정된 패턴과 동일한 패턴을 검색하여 배열 또는 `null` 문자를 반환한다.
- `String.replace( regexp, replaceText )`: 지정된 패턴과 검색하여 `replaceText`로 대체
- `RegExp.exec( testString )`: 지정된 패턴과 같은 패턴을 검색하여 배열 또는 `null` 문자를 반환한다.
- `RegExp.test( testString )`: 지정된 패턴과 같은 패턴을 검색하여 검색하면 `true` 를 반환하며 그렇지 않으면 `false` 반환한다.
- `RegExp.compile( pattern, [ flags ] )`: `compile` 메서드는 script 수행 중 정규 식 개체를 컴파일한다. `compile` 메서드는 단 한번 컴파일하기 위하여 기능함수 생성자로 생성된 `RegExp` 개체와 함께 사용된다. 그래서 정규식의 반복적인 컴파일을 방지할 수 있다.

### String.match

일치하는 문자들을 배열로 반환한다. `g` 플래그가 없으면 맨 처음 찾은 하나만 반환.

```js
"ecmascript javascript".match(/script/g); // [ "script", "script" ]
```

### String.search

검색한 문자의 인덱스 반환. `g` 플래그는 무시함.

```js
"javascript".search(/va/); // 2
```

### String.replace( regexp, replaceText )

지정된 패턴과 검색하여 replaceText로 대체

```js
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

### RegExp.exec( testString )

지정된 패턴과 같은 패턴을 검색하여 배열 또는 null 문자를 반환

```js
TODO
```

### RegExp.test( testString )

지정된 패턴과 같은 패턴을 검색하여 검색하면 true를 반환하며 그렇지 않으면 false

```js
/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/.test(str)
```

## 정규 표현식에서 사용하는 특수문자

| 기호      |  설명                                                    |
|--------|--------------------------------------------------------------------------------------------------------------------------------------|
| `\`      |  바로 다음에 나오는 특수 문자를 문자열로 인식. `\\` 시퀀스는 `\`를 찾고 `\/`는 `/`를 찾는다                                                    |
| `^`      |  입력 문자열의 시작 위치를 검색. `^A` 는 검색하고자 하는 문장의 시작문자가 A인지를 검사                                                      |
| `$`      |  입력 문자열의 끝 위치를 검색. `A$` 는 검색하고자 하는 문장의 마지막문자가 A인지를 검사                                                      |
| `*`      |  0개 이상의 문자를 검색(모든 것이라는 의미 → `{0,}` 같은 의미). `cg*`는 'c', 'cginjs' 등..                                                 |
| `+`      |  1개 이상의 문자를 검색(`{1,}` 같은 의미). `cg+`는 'cg', 'cginjs' 등이지만 'c'는 아니다.                                                   |
| `?`      |  0 또는 1개의 문자 의미.(`{0,1}` 같은 의미). `C?j` 라면 C라는 문자와 j라는 문자 사이에 문자가 0개 또는 1개 가 들어갈 수 있다는 의미           |
| `.`      |  `\n`을 제외한 모든 단일 문자를 검색. `\n`을 포함한 모든 문자를 찾으려면 `[.\n]` 패턴을 사용                                               |
| `()`     |  한번 match를 수행해서 나온 결과를 기억함. `/(cnj)/` 는 cnj라는 단어를 검색한 후, 그 단어를 배열등과 같은 저장장소에 남겨두어 나중에 다시 호출할 수 있도록 함. `(.)\1`은 연속적으로 나오는 동일한 문자 두 개를 찾는다. |
| `|`      |  부분합연산(OR)                                                                                                                       |
| `{n}`    |  정확히 n개의 문자(n은 음이 아닌 정수)                                                                                                  |
| `{n,}`   |  n개 이상의 문자 검색(n은 음이 아닌 정수) 예) `c{2,}`는 'cnj'의 'c'는 찾지 않지만 'bcccccccccf'의 모든 c는 검색                            |
| `{n, m}` |  최소 n개에서 최대 m개 검색. `b{1,4}`은 'bcccccccccf'의 처음 네 개의 c를 검색하며 쉼표와 숫자 사이에는 공백을 넣을 수 없다.                  |
| `[xyz]`  |  괄호 안의 문자 중 하나를 검색. 예를 들어 `[a-z]`라면 a부터 z까지의 모든 문자검색. `[abc]`는 'cnj'의 'c'를 검색                               |
| `[^xyz]` |  제외 문자 집합. `[^abc]`는 'acn'의 'n'를 검색                                                                                          |
| `x|y`    |  x 또는 y를 검색. `c|cginjs`는 'c' 또는 'cginjs'를 검색                                                                                 |
| `[a-z]`  |  문자 범위. 지정한 범위 안의 문자 검색. `[a-z]`는 a부터 z사이의 모든 문자(여기선 소문자) 검색                                               |
| `[^a-z]` |  제외문자 범위 검색. `[^a-z]`는 'a'부터 'z' 사이에 없는 모든 문자를 검색                                                                  |
| `[\b]`   |  백스페이스 검색                                                                                                                       |
| `\b`     |  단어와 공백 사이의 위치를 검색. `er\b`는 'never'의 'er'는 찾지만 'verb'의 'er'는 찾지 않는다.                                             |
| `\B`     |  단어와 비경계를 찾는다. `er\B`는 'verb'의 'er'는 찾지만 'never'의 'er'는 찾지 않는다.                                                    |
| `\cX`    |  X가 나타내는 제어 문자를 찾는다. `\cM`은 `Control-M` 즉, 캐리지 리턴 문자를 찾는다.                                                          |
| `\d`     |  0부터 9까지의 아라비아 숫자와 찾는다. `[0-9]`와 같은 의미                                                                                  |
| `\D`     |  비 숫자 문자를 찾는다. `[^0-9]`와 같은 의미                                                                                               |
| `\f`     |  폼피드 문자(form-feed)를 검색(`\x0c`와 `\cL`과 같은 의미)                                                                                   |
| `\n`     |  linefeed(줄 바꿈 문자)를 검색(`\x0a`와 `\cJ`와 같은 의미)                                                                                   |
| `\r`     |  캐리지 리턴 문자를 검색(`\x0d`와 `\cM`과 같은 의미)                                                                                          |
| `\s`     |  공백, 탭, 폼피드 등의 공백을 검색(`[ \t\n\r\f\v]`와 같은 의미)                                                                             |
| `\S`     |  `\s`가 아닌 문자(공백이 아닌 문자)를 검색. `[^ \t\n\r\f\v]`와 같은 의미                                                                     |
| `\t`     |  탭 문자를 검색(`\x09`와 `\cI`와 같은 의미)                                                                                                  |
| `\v`     |  수직 탭 문자를 검색(`\x0b`와 `\cK`와 같은 의미)                                                                                              |
| `\w`     |  밑줄을 포함한 모든 단어 문자를 검색(`[A-Za-z0-9_]`와 같은 의미)                                                                          |
| `\W`     |  문자가 아닌 요소, 즉 % 등과 같은 특수 문자를 의미함(`[^A-Za-z0-9_]`와 같은 의미)                                                          |
| `\n`     |  n은 마지막 일치하는 문장                                                                                                                |
| `\xn`    |  n을 검색 여기서 n은 16진수 이스케이프 값으로 16진수 이스케이프 값은 정확히 두 자리여야 한다. `\x41`은 'A'를 찾고 `\x041`은 `\x04`와 '1'과 같다.|
| `\num`   |  num을 찾는다.(num은 양의 정수)                                                                                                          |
| `\nm`    |  8진수 이스케이프 값이나 역 참조를 나타낸다. `\nm` 앞에 최소한 nm개의 캡처된 부분식이 나왔다면 nm은 역참조이며 `\nm` 앞에 최소한 n개의 캡처가 나왔다면 n은 역참조이고 뒤에는 리터럴 m이 온다. 이 두 경우가 아닐 때 n과 m이 0에서 7 사이의 8진수이면 `\nm`은 8진수 이스케이프 값 nm을 찾는다. |
| `\nml`   |  n이 0에서 3 사이의 8진수이고 m과 l이 0에서 7 사이의 8진수면 8진수 이스케이프 값 nml을 찾는다.                                               |
| `\un`    |  n은 4 자리의 16진수로 표현된 유니코드 문자. `\u00A9`는 저작권 기호(ⓒ)를 찾는다.                                                             |
