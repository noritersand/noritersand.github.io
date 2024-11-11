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
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [RegExp \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [Regular expressions \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [RegExr](https://regexr.com/)
- [https://regexr.com/5ml92](https://regexr.com/5ml92)
- [regular expressions 101](https://regex101.com/)
- [REGEXPER](https://regexper.com/)
- [regular expression library](http://regexlib.com/DisplayPatterns.aspx)
- [RegexOne](https://regexone.com/)
- [http://blog.outsider.ne.kr/360](http://blog.outsider.ne.kr/360)


## 개요

정규 표현식이란 문자열에서 특정한 캐릭터 조합을 찾아내기 위한 패턴이다. 줄여서 정규식이라고도 한다.


## 초기화

#### 리터럴 표기법 Literal Notation

```
/pattern/
/pattern/flags
```

```js
/MSIE/.test(window.navigator.userAgent);
```

#### Constructor

```
new RegExp(pattern)
new RegExp(pattern, flags)
RegExp(pattern)
RegExp(pattern, flags)
```

```js
var re = RegExp('ab+c', 'i');
```

생성자 방식은 기본적으로 리터럴 표기법과 결과는 같다. 이 방법은 정규식 패턴을 변수로 다뤄야할 때 유용하다:

```js
let value = 'abc';
let from = 'C';
let to = '씨';
value = value.replace(RegExp(from, 'gi'), to); // 'ab씨'
```


## flag

- `g`: 전역 검색 플래그. 대상 문자열에서 패턴과 일치하는 모든 부분을 찾는다. 이 플래그가 없으면 정규식 패턴과 일치하는 첫 번째 문자만 찾고 검색을 중지한다.
- `i`: 대/소문자 무시.
- `gi`: 대/소문자 무시하고 전역 검색.
- `m`: 멀티 라인 검색. `g` 플래그가 문자열 전체에서 검색이라 이 플래그와 기능이 겹치는 것 같지만 다르다. 설명에 따르면 시작 위치와 관련된 검색일 때 `m` 플래그가 있으면 각 줄의 시작 위치에서 새로 검색을 시작한다.
- `y`: Sticky 모드 활성화 플래그. `RegExp` 인스턴스를 사용할 때만 유효하다. `g` 플래그와 비슷하지만 약간 다르다. 저 아래에서 추가 설명함.


## 정규 표현식과 함께 사용하는 함수

### String.prototype.search()

```
string.search(pattern)
```

정규식 패턴에 첫 번째로 일치하는 문자열의 인덱스를 반환하며, 패턴과 일치하는 문자열이 하나도 없으면 `-1`을 반환한다.

검색한 문자의 인덱스 반환. 글로벌`g` 플래그는 무시된다(하나 찾으면 반환하며 종료). 

```js
var str = 'javascript';
str.search(/vas/); // 2
str.search(/qwe/); // -1
```

### String.prototype.match()

```
string.match(pattern)
```

패턴과 일치하는 문자들을 배열로 반환한다. 일치하는 문자가 없으면 `null`을 반환한다. `g` 플래그가 없으면 맨 처음 찾은 하나만 반환하고 종료한다.

```js
var str = 'ecmascript javascript';
str.match(/script/); // [ "script" ]
str.match(/script/g); // [ "script", "script" ]
str.match(/foobar/g); // null
```

### String.prototype.replace()

```
string.replace(pattern, replacement)
```

패턴과 일치하는 문자를 `replacement`로 대체한다.

```js
'123'.replace('2', 'two'); // "1two3" 
'a|b|c'.replace(/\|/g, ''); // "abc"
'abcde'.replace(/\w/g, 'f'); // "fffff"
```

### String.prototype.replaceAll()

```
string.replaceAll(pattern, replacement)
```

패턴과 일치하는 모든 문자를 `replacement`로 대체한다. `pattern`이 정규식일 때 `g` 플래그가 없으면 `TypeError`가 발생한다.

```js
'aaabbaaa'.replaceAll(/a/g, ''); // "bb"
'a'.replaceAll(/a/, ''); // Uncaught TypeError: replaceAll must be called with a global RegExp
```

### RegExp.prototype.test()

```
regexp.test(testString)
```

일치하는 패턴이 있으면 `true`를, 그렇지 않으면 `false` 반환한다.

```js
/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/.test('한글'); // true
```

`g` 플래그가 주어진 `Regex` 인스턴스는 `test()`를 호출할 때 내부에서 `lastIndex`\*를 마지막 검색 위치의 바로 다음 인덱스로 업데이트한다:

\* `RegExp.prototype.lastIndex`: 다음 검색 시작 위치를 저장하는 `RegExp` 인스턴스 프로퍼티

```js
var regex = /a/g;

console.log(regex.lastIndex); // 0 (초기값)

console.log(regex.test('a'));  // true (검색 성공)
console.log(regex.lastIndex);  // 1 (검색 위치의 다음 인덱스로 변경됨)

console.log(regex.test('a'));  // false (이전 검색이 끝난 위치에서 다시 검색해서 false)
console.log(regex.lastIndex);  // 0 (일치하는 게 없으면 0으로 재설정됨)

console.log(regex.test('a'));  // true (다시 처음부터 검색해서 true)
console.log(regex.lastIndex);  // 1
```

### RegExp.prototype.exec()

```
regexp.exec(testString)
```

지정된 패턴과 일치하는 패턴을 검색하여 배열을 반환하는데, 일치하는 패턴이 없으면 `null`을 반환한다. 

```js
var rxp = RegExp(/\d+/);
console.log(rxp.exec('1234')); // Array [ "1234" ]
console.log(rxp.exec('abc')); // null

var rxp2 = RegExp(/\d*/);
console.log(rxp2.exec('1234')); // Array [ "1234" ]
console.log(rxp2.exec('abc')); // Array [ "" ] // 빈 문자열 배열이 반환되는 이유는 0개 이상의 숫자를 의미하는 정규식 때문
```

`g` 플래그가 있으면 단순 패턴 일치 검색이 아니게 된다.

```js
var regx = RegExp(/\w/, 'g');
var str = 'ab';
console.log(regx.exec(str)); // Array [ "a" ]
console.log(regx.lastIndex); // 1

console.log(regx.exec(str)); // Array [ "b" ]
console.log(regx.lastIndex); // 2

console.log(regx.exec(str)); // null
console.log(regx.lastIndex); // 0
```

글로벌 모드일 때 패턴과 일치하는 문자를 찾으면 `exec()`는 (`RegExp.prototype.test()`와 마찬가지로) 찾은 문자열 배열을 반환하고 `regexp.lastIndex`를 다음 검색을 시작할 위치(인덱스)로 변경한다. 문자열의 끝에 도달하면 `null`을 반환하고 `lastIndex` 프로퍼티는 `0`으로 변경된다.

위 코드를 예시로 설명하면, 첫 번째 `exec()` 호출 후 `lastIndex`는 'a'의 다음 인덱스인 `1`이다. 두 번째 호출 후에 `lastIndex`는 'b'의 다음 인덱스인 `2`가 되고, 세 번째 호출에선 더 찾을 문자가 없으로 `lastIndex`는 `0`이 되는 것이다.

`lastIndex`는 강제로 변경할 수 있다. 다음 예시를 보자:

```js
var regx = RegExp(/\w/, 'g');
var str = 'ab';

regx.lastIndex = 1
console.log(regx.exec(str)); // Array [ "b" ]

regx.lastIndex = 1
console.log(regx.exec(str)); // Array [ "b" ]
```

⚠️ 특정 브라우저에서는 콘솔창에 붙여넣은 표현식을 미리 실행한다. 그러니까 작성한 메서드가 실제로는 두 번 이상 호출된다는 말이다(특히 파이어폭스가 그렇다). 따라서 `regexp.test()` 혹은 `regexp.exec()`는 콘솔창에서 테스트 하지 말자.

#### `y` 스티키(Sticky) 플래그와 `g` 글로벌 플래그의 차이

`y` 플래그는 스티키 모드를 활성화하는데, `g` 플래그와 비슷하게 작동하지만, 패턴 일치가 정확히 `lastIndex` 부터 발생하지 않으면 검색 실패로 간주한다.

```js
var str = 'abcde';

var regx = /[bd]/g;
regx.lastIndex = 2;
console.log(regx.exec(str)); // Array [ "d" ]
console.log(regx.lastIndex); // 4

var regx2 = /[bd]/y;
regx2.lastIndex = 2;
console.log(regx2.exec(str)); // null
console.log(regx2.lastIndex); // 0
```

문자열 'abcde'에서 'b' 혹은 'd'를 찾는 패턴일 때 `g`와 `y`의 차이를 보여주는 코드다. `g` 플래그는 'c'부터 찾기 시작해서 'd'를 반환한다. 이에 비해 `y` 플래그는 'c'가 'b' 혹은 'd'가 아니므로 바로 `null`을 반환한다.

### 🚧 RegExp.compile()

```
regexp.compile(pattern, [flags])
```

과거에 정규식을 컴파일할 때 사용하던 메서드. 현재는 인스턴스 생성 시 자동으로 컴파일한다. 따라서 없는 메서드로 취급하면 된다.


## 정규 표현식에서 사용하는 특수문자

MDN에서는 [Assertions, Character classes, Groups and backreferences, Quantifiers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#using_special_characters)로 나누어 설명한다.

- Assertions: `^, $, \b, \B, x(?=y), x(?!y), (?<=y)x, (?<!y)x`
- Character classes: `[xyz], [^xyz], ., \d, \D, \w, \W, \s, \S, \t, \r, \n, \v, \f, [\b], \0, \cX, \xhh, \uhhhh, \u{hhhh}, x|y`
- Groups and backreferences: `(x), (?<Name>x), (?:x), \n, \k<Name>`
- Quantifiers: `x*, x+, x?, x{n}, x{n,}, x{n,m}`

### Assertions

[Assertions - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Assertions)

#### `^`

입력 문자열의 시작 위치를 검색. `^A` 는 검색하고자 하는 문장의 시작문자가 A인지를 검사. `^qwe`는 정확히 'qwe'로 시작하는지를 검사한다. 여기서 문장은 문자열 전체를 의미할 수도 있고, 줄 바꿈 문자를 기준으로 한 각 라인일 수도 있다. 이것은 정규식 플래그에 따라 달라지는데, 멀티 라인`m` 플래그가 있으면 각 라인마다 지정한 문자로 시작하는지를 검사한다.

검색할 때 자주 쓰인다. 예를 들어 (멀티 라인 플래그가 있다고 가정하고) `\n`은 모든 줄 바꿈 문자인데 `^\n`은 라인의 시작이 줄 바꿈인 것, 그러니까 줄 바꿈 문자만 있는 줄을 의미한다. `##`는 `#` 두 개가 포함된 모든 문자를 찾지만, `^## `라고 작성하면 `#` 두 개로 시작하고 공백 하나가 바로 이어지는 문자를 찾는다. (문서 내 모든 헤더2를 찾을 때 쓴다)

```js
// 라인이 "x"로 시작하는지 검사
/^x/
```

#### `$`

입력 문자열의 끝 위치를 검색. `A$` 는 검색하고자 하는 문장의 마지막문자가 A인지를 검사한다.

`qwe$`는 문자열이나 라인이 정확히 'qwe'로 끝나는지를 검사한다. 위의 `^`와 조합하면 `^qwe$`인데 문자열이나 라인에 'qwe'만 존재하는지를 검사한다.

```js
// 라인이 "x"로 끝나는지 검사
/x$/
```

#### `\b`

단어 경계(word boundary) 위치가 일치하는지 검색한다. 단어 경계란 단어 구성 문자(word character)\* 뒤에 공백이나 쉼표 같은 비단어 구성 문자(non-word character, 비문자라고 하기도 함)가 이어지는 것을 말한다.

- 정규식 `\babc\b`는 '(공백)abc,'과 일치하지만 'qabce,'는 일치하지 않는다.
- 정규식 `er\b`는 'never'에서 'er'은 찾지만 'verb'의 'er'은 일치하지 않는다.

\* 정규식에서 한글 같은 유니코드는 '단어 구성 문자'로 취급되지 않으니 주의할 것.

#### `\B`

`\b`의 반대격. 단어와 비경계를 찾는다. 

정규식이 `er\B`일 때 'verb'의 'er'과 일치하고, 'never'에서의 'er'은 일치하지 않는다.

#### `x(?=y)`

이름은 긍정형 전방 탐색(Positive lookahead)이다. `y`가 `x` 뒤에 있는 경우만 일치로 판단한다.

정규식 `foo(?=bar)`는 'bar'가 바로 뒤에 오는 'foo'와 일치한다.

ℹ️ lookahead나 lookbehind는 탐색 조건으로 사용하되 선택은 하지 않는다는 특징이 있다.

#### `x(?!y)`

이름은 부정형 전방 탐색(Negative lookahead). `y`가 `x` 뒤에 오지 않는 경우만 일치로 판단한다.

`foo(?!bar)`는 'bar'가 바로 뒤에 오지 않는 `foo`, 그러니까 `foobar`를 제외한 모든 `foo`와 일치한다.

#### `(?<=y)x`

이름은 긍정형 후방 탐색(Positive lookbehind). `y`가 `x` 앞에 있는 경우만 일치로 판단한다.

`(?<=foo)bar`는 정확히 'foo'가 앞에 있는 'bar'와 일치하는 패턴이다. 'foobar'만 일치하고, 'bar', 'fobar', 'fooobar', 'abar'는 일치하지 않는다.

#### `(?<!y)x`

이름은 부정형 후방 탐색(Negative lookbehind). `y`가 `x` 앞에 오지 않는 경우만 일치로 판단한다.

`(?<!foo)bar`는 'foo'로 시작하지 않는 'bar'와 일치하는 패턴이다. 

### Character classes

[Character classes - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Character_classes)

#### `[xyz]`

괄호 안의 문자 중 하나를 검색. 예를 들어 정규식 `[a-z]`는 'a'부터 'z'까지를 의미한다. `abc`는 정확히 'abc'를 찾지만 `[abc]`는 'a', 'b', 'c'를 각각 찾는다. `[lmn]`은 'cnj'에서 'n'을 찾는 패턴이다.

#### `[^xyz]`

제외 문자 집합. `[^abc]`는 'acn'의 'n'과 일치함

#### `[a-z]`

문자 범위. 지정한 범위 안의 문자 검색. `[a-z]`는 a부터 z사이의 모든 문자(여기선 소문자) 검색

#### `[^a-z]`

제외문자 범위 검색. `[^a-z]`는 'a'부터 'z' 사이에 없는 모든 문자를 검색

#### `.`

줄 바꿈`\n`을 제외한 모든 단일 문자를 검색. 줄 바꿈 문자를 포함한 모든 문자를 찾는 패턴은 `[.\n]`. `*`과 조합한 `.*`로 길이 제한이 없는 모든 문자 일치 패턴으로 자주 사용된다. `.*foo.*`는 'foo'를 포함하는 라인 한 줄 전부를 찾는다.

#### `\d`

0부터 9까지의 아라비아 숫자와 찾는다. `[0-9]`와 같음 

#### `\D`

비 숫자 문자를 찾는다. `[^0-9]` 혹은 `[^\d]`와 같음 

#### `\w`

단어 구성 문자(word character). 밑줄을 포함한 기본 라틴 알파벳의 모든 영숫자 문자(alphanumeric character)를 찾는다. `[A-Za-z0-9_]`와 같음

#### `\W`

`\w`의 반대. 기본 라틴 알파벳의 단어 구성 문자가 아닌 모든 문자. `[^A-Za-z0-9_]`와 같음

#### `\s`

공백, 탭, 폼피드 등의 공백을 검색(`[ \t\n\r\f\v]`와 같음)

#### `\S`

`\s`가 아닌 문자(공백이 아닌 문자)를 검색. `[^ \t\n\r\f\v]`와 같음 

#### `\t`

탭 문자를 검색(`\x09`와 `\cI`와 같음)

#### `\r`

캐리지 리턴 문자를 검색(`\x0d`와 `\cM`과 같음)

#### `\n`

줄 바꿈 문자(linefeed)를 검색(`\x0a`와 `\cJ`와 같음)

#### `\nm`

8진수 이스케이프 값이나 역 참조를 나타낸다. 

`\nm` 앞에 최소한 `nm`개의 캡처된 부분식이 나왔다면 `nm`은 역참조이며 `\nm` 앞에 최소한 `n`개의 캡처가 나왔다면 `n`은 역참조이고 뒤에는 리터럴 `m`이 온다. 이 두 경우가 아닐 때 `n`과 `m`이 `0-7` 사이의 8진수면 `\nm`은 8진수 이스케이프 값 `nm`을 찾는다.

#### `\nml`

`n`이 `0-3` 사이의 8진수이고 `m`과 `l`이 `0-7` 사이의 8진수면 8진수 이스케이프 값 `nml`을 찾는다.

#### `\v`

수직 탭 문자를 검색(`\x0b`와 `\cK`와 같음)

#### `\f`

폼피드 문자(form-feed)를 검색(`\x0c`와 `\cL`과 같음)

#### `[\b]`

백스페이스 검색

#### `\cX`

X가 나타내는 제어 문자를 찾는다. `\cM`은 `Control-M`, 즉 캐리지 리턴 문자를 찾는다.

#### `\xhh`

`hh`을 검색 여기서 `hh`은 정확히 두 자리의 16 진수 이스케이프 값이다. `\x41`은 'A'를 찾고 `\x041`은 `\x04`와 '1'과 같다.

#### `\uhhhh`, `\u{hhhh}`

`hhhh`는 4 자리의 16진수로 표현된 유니코드 문자. `\u00A9`는 저작권 기호(ⓒ)를 찾는다.

#### `x|y`

`x` 또는 `y`를 검색한다는 뜻이다. 파이프`|`는 *Disjunction* 혹은 *Alternation*이라 부르며 부분합연산(OR)을 수행한다. 예를 들어 `c|cginjs`는 'c' 또는 'cginjs'와 일치한다.

부분적인 조건을 판단하려면 소괄호와 같이 쓴다. `fla(?:vor|me)`는 `flavor`와 `flame`을 찾는다. (여기서 `?:`는 소괄호`()`로 그룹을 만들고 싶지만 캡처는 필요 없을 때 사용함)

### Groups and backreferences

[Groups and backreferences - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Groups_and_backreferences)

#### `()`

소괄호`()`는 Grouping(그룹화)와 Capturing(캡처화)에 사용된다. 그룹화는 어떤 패턴을 하나의 그룹으로 묶는 것, 캡처화는 패턴과 일치하는 문자를 재사용하기 위해 추출 혹은 별도로 기억하는 것을 말한다. `()`로 그룹을 만들면 이 그룹은 자동으로 캡처 된다.

그룹화는 세 가지 패턴이 있다:

- `(x)`: *Capturing group*
- `(?<Name>x)`: *Named capturing group*
- `(?:x)`: *Non-capturing group*. 캡처가 필요 없는 그룹을 만들 때 쓴다.

재사용 방법은 여러가지인데, 예를 들어 `RegExp.exec()`는 `()`로 묶인 그룹을 배열로 반환한다:

```js
/(\d{3})-(\d{4})-(\d{4})/.exec('010-1234-5678'); // Array(4) [ "010-1234-5678", "010", "1234", "5678" ]
```

다른 방법으로 정규식 내부 혹은 `String.prototype.replace()` 메서드에서 capturing된 그룹을 참조하는 방식이 있다. 이것은 *그룹 참조(group reference)*라고 한다. **TODO** 맞는지 확인

정규식 내에서의 그룹 참조는 `\숫자` 형태로 작성한다:

```js
'aabbcc'.replace(/(.+)\1/, 'a'); // abbcc
```

`String.replace(regexp, replaceText)`의 `replaceText`에선 그룹 참조를 `$숫자` 형태로 작성할 수 있다:

```js
// A 혹은 B 혹은 C로 시작하고(이 부분을 그룹1로 묶음) 숫자로 이어지는 패턴을 검색, 그룹1은 그대로 두고 숫자부분은 'R'로 치환
'A1 B2 C3'.replace(/(A|B|C)\d/gm, '$1R'); // "AR BR CR"

// 알파벳으로 시작하고 숫자(이 부분을 그룹1로 묶음)로 이어지는 패턴을 검색, 알파벳은 'R'로 치환하고 숫자는 그대로 출력
'A1 B2 C3'.replace(/[A-Z](\d)/gm, 'R$1'); // "R1 R2 R3" 

// 소괄호 세 개를 '$숫자'로 기억해서 숫자넷-숫자둘-숫자둘 형식으로 replace
'20220301'.replace(/(\d{4})(\d{2})(\d{2})/, '$1-$2-$3');

// 천 단위마다 쉼표 추가
'1000000'.replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
```

### Quantifiers

[Quantifiers - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Quantifiers)

Quantifiers는 찾으려는 문자나 문자열 패턴이 몇 번 반복될 것인지를 지정한다. 항상 Quantifiers가 아닌 패턴과 조합해 사용한다.

`x*`, `x+`, `x?`, `x{n}`, `x{n,}`, `x{n,m}`

#### `x*`

0개 이상의 문자, 즉 길이 제한 없이 모든 문자를 검색한다. `{0,}`라고 작성한 것과 같다. 항상 어떤 문자 다음에 위치해야 한다.

`por*`는 'po' 다음에 'r'이 0번 이상 존재하는 문자열을 찾는다. 0번 이므로 'r'이 없는 경우도 포함해 'po', 'por', 'porr' 등을 찾는다.

줄 바꿈을 제외한 모든 문자를 의미하는 `.`과 조합하여 모든 문자열 검색에 활용할 수 있다. 'qwer'을 포함하는 라인 전체를 선택하려면 `.*qwer.*`라고 작성한다.

#### `x+`

`x`가 1개 이상인 패턴을 의미한다(`{1,}` 같음). 'x'는 반드시 하나 이상 있어야 하는 필수적인 요소다. `w+h`는 'wh', 'wwh', 'wwwh' 같은 'h'앞에 'w'가 1개 이상이 있는 패턴과 일치한다.

#### `x?`

`x`가 0개 또는 1개인 패턴을 의미한다(`{0, 1}`과 같음). 'x'는 있을 수도 있고 없을 수도 있는 선택적인 요소다. `w?h`는 'wh', 'h'만 일치하는 패턴이다. 1개만 있는지 판단하기 때문에 'wwwh'처럼 'w'가 여러 개 있어도 'wh'만 선택한다.

`.+`와 조합한 `.+?` 패턴이 있다. `.+?`는 lazy quantifier 혹은 non-greedy, reluctant, minimal, ungreedy quantifier 등으로 알려져 있다. 간단히 요약하면 가능한 모든 것을 찾는 것과, 한 개 찾으면 관두는 것 정도의 차이다. 예를 들어 'waaagh'에서  `wa.+`는 'waaagh' 전체를 선택하지만 `wa.+?`는 'waa'까지만 선택한다. (모든 문자`.`를 만족하는 'a' 하나만 더 있으면 일치하는 패턴이기 때문)

관련 글: [https://stackoverflow.com/questions/14213848/difference-between-and](https://stackoverflow.com/questions/14213848/difference-between-and)

어쨋든 `.+?`는 최소 한 개 이상의 문자 중 가장 짧은 것을 의미한다. 이 패턴은 얼핏 보면 `.{1}`와 같은 것처럼 보이는데, 뒤에 정규식이 조금 더 붙으면 얘기가 달라진다. 가령 'waaagh'에서 `wa.{1}h` 패턴과 일치하지 않지만 `wa.+?h` 패턴과는 일치한다. (무조건 하나를 의미하는 게 아니라 '한 개 이상인 것 중 가장 짧은 것'이기 때문이다)

#### `x{n}`

정확히 `n`개의 문자(`n`은 음수가 아닌 정수)

#### `x{n,}`

`n`개 이상의 문자 검색(`n`은 음수가 아닌 정수).

`c{2,}`는 2개 이상인 것을 찾기 때문에 'cnj'에선 아무것도 일치하지 않지만 'bcccccccccf'에선 'c' 9개와 일치한다.

#### `x{n,m}`

최소 `n`개에서 최대 `m`개 검색. `b{1,4}`은 'bcccccccccf'의 처음 네 개의 `c`와 일치한다. 쉼표와 숫자 사이에는 공백을 넣을 수 없음.
