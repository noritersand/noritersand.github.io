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

#### 관련 문서

- [\[MDN\] RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [\[MDN\] Regular expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [regular expressions 101](https://regex101.com/)
- [RegExr](https://regexr.com/)
- [REGEXPER](https://regexper.com/)
- [http://blog.outsider.ne.kr/360](http://blog.outsider.ne.kr/360)
- [http://regexlib.com/DisplayPatterns.aspx](http://regexlib.com/DisplayPatterns.aspx)


## 개요

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

이 방법은 정규식 패턴을 변수로 다뤄야할 때 유용하다:

```js
let value = 'abc';
let from = 'C';
let to = '씨';
value = value.replace(new RegExp(from, 'gi'), to); // 'ab씨'
```


## flag

- `g`: 전역 검색 플래그. 문자열 전체에서 검색한다. 이 플래그가 없으면 메서드에 따라 첫 번째 검색 결과만 반환함.
- `i`: 대/소문자 무시.
- `gi`: 대/소문자 무시하고 전역 검색.
- `m`: 멀티 라인 검색. `g` 플래그가 문자열 전체에서 검색이라 이 플래그와 기능이 겹치는 것 같지만 다르다. 설명에 따르면 시작 위치와 관련된 검색일 때 `m` 플래그가 있으면 각 줄의 시작 위치에서 새로 검색을 시작한다.
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
'123'.replace('2', 'two'); // "1two3" 
```

### RegExp.exec( testString )

지정된 패턴과 같은 패턴을 검색하여 배열 또는 null 문자를 반환

```js
/\d*/.exec('1234'); // Array [ "1234" ]
```

### RegExp.test( testString )

지정된 패턴과 같은 패턴을 검색하여 검색하면 true를 반환하며 그렇지 않으면 false

```js
/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/.test('한글'); // true
```


## 정규 표현식에서 사용하는 특수문자

MDN에서는 [Assertions, Character classes, Groups and backreferences, Quantifiers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#using_special_characters)로 나누어 설명한다.

TODO 분류할 것

### Assertions
### Character classes
### Groups and backreferences
### Quantifiers

#### `\`

바로 다음에 나오는 특수 문자를 문자열로 인식. `\\` 시퀀스는 `\`를 찾고 `\/`는 `/`를 찾는다.

#### `^`

입력 문자열의 시작 위치를 검색. `^A` 는 검색하고자 하는 문장의 시작문자가 A인지를 검사.

`^qwe`는 어떤 문자열이나 라인이 정확히 'qwe'로 시작하는지를 검사한다.

#### `$`

입력 문자열의 끝 위치를 검색. `A$` 는 검색하고자 하는 문장의 마지막문자가 A인지를 검사.

`qwe$`는 문자열이나 라인이 정확히 'qwe'로 끝나는지를 검사한다. 위의 `^`와 조합하면 `^qwe$`인데 문자열이나 라인에 'qwe'만 존재하는지를 검사한다.

#### `.`

줄 바꿈`\n`을 제외한 모든 단일 문자를 검색. 줄 바꿈 문자를 포함한 모든 문자를 찾으려면 `[.\n]`

#### `*`

0개 이상의 문자, 즉 길이 제한 없이 모든 문자를 검색한다. `{0,}`라고 작성한 것과 같다. 항상 어떤 문자 다음에 위치해야 한다.

`por*`는 `po` 다음에 `r`이 0번 이상 존재하는 문자열을 찾는다. 0번 이므로 r이 없는 경우도 포함해 `po`, `por`, `porr` 등을 찾는다.

줄 바꿈을 제외한 모든 문자를 의미하는 `.`과 조합하여 모든 문자열 검색에 활용할 수 있다. `qwer`을 포함하는 라인 전체를 선택하려면 `.*qwer.*`라고 작성한다.

#### `+`

1개 이상의 문자를 검색(`{1,}` 같은 의미). `cg+`는 'cg', 'cginjs' 등이지만 'c'는 아니다.

#### `?`

0 또는 1개의 문자 의미.(`{0,1}` 같은 의미). `C?j` 라면 C라는 문자와 j라는 문자 사이에 문자가 0개 또는 1개 가 들어갈 수 있다는 의미.

#### `|`

부분합연산(OR)

#### `{n}`

정확히 n개의 문자(n은 음수가 아닌 정수)

#### `{n,}`

n개 이상의 문자 검색(n은 음수가 아닌 정수).

`c{2,}`는 'cnj'의 'c'는 찾지 않지만 'bcccccccccf'의 모든 c는 검색

#### `{n, m}`

최소 n개에서 최대 m개 검색. `b{1,4}`은 'bcccccccccf'의 처음 네 개의 c를 검색하며 쉼표와 숫자 사이에는 공백을 넣을 수 없다.

#### `[xyz]`

괄호 안의 문자 중 하나를 검색. 예를 들어 `[a-z]`라면 a부터 z까지의 모든 문자검색. `[abc]`는 'cnj'의 'c'를 검색

#### `[^xyz]`

제외 문자 집합. `[^abc]`는 'acn'의 'n'를 검색

#### `x|y`

x 또는 y를 검색. `c|cginjs`는 'c' 또는 'cginjs'를 검색

#### `[a-z]`

문자 범위. 지정한 범위 안의 문자 검색. `[a-z]`는 a부터 z사이의 모든 문자(여기선 소문자) 검색

#### `[^a-z]`

제외문자 범위 검색. `[^a-z]`는 'a'부터 'z' 사이에 없는 모든 문자를 검색

#### `[\b]`

백스페이스 검색

#### `\b`

단어 경계(word boundary) 위치가 일치하는지 검색한다. 단어 경계란 단어 문자(word-character) 뒤에 공백이나 쉼표같은 비단어 문자가 이어지는 것을 말한다.

- 정규식 `\babc\b`는 `(공백)abc,`를 찾지만 `qabce,`는 찾지 않는다.
- 정규식 `er\b`는 `never`에서 `er`은 찾지만 `verb`의 `er`은 찾지 않는다.

#### `\B`

단어와 비경계를 찾는다. `er\B`는 'verb'의 'er'는 찾지만 'never'의 'er'는 찾지 않는다.

#### `\cX`

X가 나타내는 제어 문자를 찾는다. `\cM`은 `Control-M`, 즉 캐리지 리턴 문자를 찾는다.

#### `\d`

0부터 9까지의 아라비아 숫자와 찾는다. `[0-9]`와 같은 의미 

#### `\D`

비 숫자 문자를 찾는다. `[^0-9]` 혹은 `[^\d]`와 같은 의미 

#### `\f`

폼피드 문자(form-feed)를 검색(`\x0c`와 `\cL`과 같은 의미)

#### `\n`

줄 바꿈 문자(linefeed)를 검색(`\x0a`와 `\cJ`와 같은 의미)

#### `\r`

캐리지 리턴 문자를 검색(`\x0d`와 `\cM`과 같은 의미)

#### `\s`

공백, 탭, 폼피드 등의 공백을 검색(`[ \t\n\r\f\v]`와 같은 의미)

#### `\S`

`\s`가 아닌 문자(공백이 아닌 문자)를 검색. `[^ \t\n\r\f\v]`와 같은 의미 

#### `\t`

탭 문자를 검색(`\x09`와 `\cI`와 같은 의미)

#### `\v`

수직 탭 문자를 검색(`\x0b`와 `\cK`와 같은 의미)

#### `\w`

밑줄을 포함한 모든 단어 문자를 검색(`[A-Za-z0-9_]`와 같은 의미)

#### `\W`

문자가 아닌 요소, `%` 같은 특수 문자를 의미함(`[^A-Za-z0-9_]`와 같은 의미)

#### `\n`

n은 마지막 일치하는 문장 

#### `\xn`

n을 검색 여기서 n은 16진수 이스케이프 값으로 16진수 이스케이프 값은 정확히 두 자리여야 한다. `\x41`은 'A'를 찾고 `\x041`은 `\x04`와 '1'과 같다.

#### `\num`

num을 찾는다.(num은 양의 정수)

#### `\nm`

8진수 이스케이프 값이나 역 참조를 나타낸다. 

`\nm` 앞에 최소한 nm개의 캡처된 부분식이 나왔다면 nm은 역참조이며 `\nm` 앞에 최소한 n개의 캡처가 나왔다면 n은 역참조이고 뒤에는 리터럴 m이 온다. 이 두 경우가 아닐 때 n과 m이 0에서 7 사이의 8진수면 `\nm`은 8진수 이스케이프 값 nm을 찾는다.

#### `\nml`

n이 0에서 3 사이의 8진수이고 m과 l이 0에서 7 사이의 8진수면 8진수 이스케이프 값 nml을 찾는다.

#### `\un`

n은 4 자리의 16진수로 표현된 유니코드 문자. `\u00A9`는 저작권 기호(ⓒ)를 찾는다.

#### `()`

한번 match를 수행해서 나온 결과를 기억한다. 이외에도 여러 역할을 하는데 대략 이 정도다:

- 그룹화(grouping)
- 캡처(capturing)
- 비캡처(non-capturing)
- 전방탐색(lookahead)
- 후방탐색(lookbehind)
- 그룹 참조(group reference)

**그룹화 gropuing**:

정규식의 하위 식, 그룹화 구문이라고도 한다. 부분 탐색 결과를 그룹으로 묶어서 재사용이 가능하도록 만드는 것이다.

재사용 방법은 여러가지인데, 예를 들어 `RegExp.exec()`는 `()`로 묶인 그룹을 배열로 반환한다:

```js
/(\d{3})-(\d{4})-(\d{4})/.exec('010-1234-5678'); // Array(4) [ "010-1234-5678", "010", "1234", "5678" ]
```

**그룹 참조 group reference**:

그룹을 재사용하는 방법 중 하나. 

정규식 내에서의 그룹 참조는 `\1`과 같은 식으로 작성한다:

```js
'aabbcc'.replace(/(.+)\1/, 'a'); // abbcc
```

`String.replace(regexp, replaceText)`의 `replaceText`에선 그룹 참조를 `$숫자` 형태로 작성할 수 있다:

```js
// 소괄호 세 개를 '$숫자'로 기억해서 숫자네개-숫자두개-숫자두개 형식으로 replace
'20220301'.replace(/(\d{4})(\d{2})(\d{2})/, '$1-$2-$3');

// 천 단위마다 쉼표 추가
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

나머지는 TODO
