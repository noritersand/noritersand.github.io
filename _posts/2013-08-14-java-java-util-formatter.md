---
layout: post
date: 2013-08-14 19:28:00 +0900
title: '[Java] java.util.Formatter'
categories:
  - java
tags:
  - java
  - formatter
  - string
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html](http://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html)
- [https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Formatter.html](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Formatter.html)


## format()

```java
format( String format, Object... args )
format( Locale locale, String format, Object... args )
```

- `locale`: 포맷 설정 시에 적용하는 locale(언어 및 지역 설정). `null`을 넘기면 지역화(localization) 설정을 하지 않는다.
- `format`: 출력할 문자열의 포맷
- `args`: 변환 문자와 치환할 값. 좌측부터 순서대로 작성한다.

여기서 `format`은 다음처럼 작성한다:

```java
"%[argument_index$][flags][width][.precision]conversion"
```

- `%:` 변환 문자 시작을 알리는 포맷 지시자(혹은 포맷 코드)
- `argument_index`: 인수 리스트 내에서의 인수의 위치를 나타내는 10진수
- `flags`: 출력 포맷을 변경하는 문자셋
- `-:` 왼쪽 끝에 출력 (default: 오른쪽 정렬)
- `width`: 필드 폭 혹은 고정 출력 폭을 의미한다. 지정한 길이보다 값의 길이가 적으면 나머지를 여백으로 채운다. 양수인 경우 여백이 좌측에, 음수일 땐 우측에 위치한다.
- `precision`: 실수의 소수점이하 자릿수로 `g`(`G`) 변환문자의 경우에는 결과의 자리수이며, `a`(`A`)의 변환문자는 precision을 지정할 수 없다. 또한 문자열은 가능하나 문자, 정수, 일자/시 각 인수 타입, 및 퍼센트 등에서는 precision을 적용할 수. 실수의 디폴트는 6이며 0인 경우 1로 간주함
- `conversion`: 변환 문자

```java
String.format("시작%4s끝", "값"); // "시작···값끝"
String.format("%% = %s", "percent sign."); // "% = percent sign."
String.format("크기%,d, 파일%,d개, 폴더%,d", 1024, 3, 1); // "크기1,024, 파일3개, 폴더1"
```


## argument_index

인수 리스트 내에서의 인수의 위치를 나타내는 10진수로 첫 번째 인수는 `1$`, 두 번째의 인수는 `2$`로 표시한다. 지정할 경우 기존의 인수 순서대로 치환하는 규칙은 무시한다. `%1$s`는 첫 번째 인수인 문자열 타입으로 치환한다는 뜻이다.

```java
String.format("%1$s is %1$s", "a", "b"); // 'b'는 무시되며 "a is a" 라고 출력함
```


## Flag

Flag는 서식을 지정할 때 사용하는 특수 문자다.

- `-`: 결과를 왼쪽으로 정렬한다. `width`를 생략하면 `java.util.MissingFormatWidthException`이 발생한다.
- `#`: 결과에 따라 대체 형식으로 출력한다. 대체 형식이란 정수의 16진수 표현에 항상 `0x`를 포함한다던지 등을 의미한다.
- `+`: 결과에 항상 부호(`+`, `-`)를 포함한다.
- ` `: 결과가 양수일 때 부호 자리를 공백으로 채운다.
- `0`: 결과를 0으로 채운다 (패딩). `width`를 생략하면 `java.util.MissingFormatWidthException`이 발생한다.
- `,`: 지역 언어별 구분 기호를 포함한다.
- `(`: 결과가 음수일 때 괄호로 묶어 표시한다.

```java
String.format("%-10s", "helloooo"); // "helloooo  "
String.format("%1$e, %1$#e, %2$x, %2$#x", 1234.0f, 4567); // "1.234000e+03, 1.234000e+03, 11d7, 0x11d"
String.format("%+d, %d", 123, -123); // "+123, -123"
String.format("%d, %+d, [% d], [% d]", -99, 123, 456, -456); // "-99, +123, [ 456], [-456]"
String.format("%05d", 123); // "00123"
String.format("%,d", 7654321); // "7,654,321"
String.format("%d, %(d, %(d", -123, 123, -123); // "-123, 123, (123)"
```


## Width

출력 표시값의 너비를 의미한다. 명시한 길이보다 표시할 값의 길이가 짧으면 나머지를 공백으로 채운다. 단, `-` flag가 있으면 공백을 우측으로, `0` flag가 있으면 공백 대신 `0`으로 채운다.

```java
String.format("%10s", "hi"); // "        hi"
```


## Precision

부동 소수점 숫자를 표시할 때 소수점 아래 자릿수를 얼마나 표시할 지를 결정하는 값이다. 정수일 땐 무시된다.

```java
String.format("%f", 6.5536f); // "6.553600"
String.format("%.2f", 6.5536f); // "6.55"
String.format("%.10f", 6.5536f); // "6.5535998344"
```


## 변환 문자 목록 conversion characters

변환 문자는 앞에 `%`를 붙여 '출력 형식 지정 문자' 혹은 '제어 문자'로 쓰인다. (이스케이프 시퀀스와 비슷하지만 다른 개념) 

`d`, `o`, `f`, `%`, `n`를 제외한 변환 문자가 대문자라면, 치환할 값도 대문자로 치환한다.

### 문자

#### `b` `B`

인수가 `null`이면 "false", `null`이 아니면 "true", 인수가 불리언 타입인 경우 `String.valueOf(arg)`의 결과로 치환한다. `%B`는 대문자로 치환한다.

```java
String.format("%b, %b, %B, %B", null, 123, false, Boolean.TRUE); // "false, true, FALSE, TRUE"
```

#### `h` `H`

인수가 `null`이면 "null"을, 아니면 `Integer.toHexString(arg)`의 반환값(16진수 문자로 변환된 10진수)이다. `%H`는 대문자로 치환한다.

```java
String.format("%h, %h, %H, %H, %H", null, 10, 16, 17, 28); // "null, a, 10, 11, 1C"
```

#### `s` `S`

인수가 `null`이면 "null"로 치환한다. 만약 `java.util.Formattable`의 구현체면 `arg.formatTo()`를 호출한다. 아니면 `arg.toString()`의 결과값이다.

```java
String.format("%s, %S, %s", null, "abcd", 1234); // "null, ABCD, 1234"
```

#### `c` `C`

유니코드 포인트 `arg`를 유니코드 문자로 치환한다.

```java
String.format("%c, %c, %C", 0x0067, 0x0047, 0x0069); // "g, G, I"
```

`0x0047`은 대문자인 `G`라서 `%c`로 작성해도 "G"가 됨.

### 숫자

#### `d`

정수를 10진수로 치환한다. 입력값은 `int`와 `long` 타입만 허용됨.

```java
String.format("%d", 15); // "15"
```

#### `o`

정수를 8진수로 치환한다.

```java
String.format("%o, %o, %o", 7, 8, 14); // "7, 10, 16"
```

#### `x` `X`

정수를 16진수로 치환한다.

```java
String.format("%x, %x, %X", 16, 15, 15); // "10, f, F"
```

#### `e` `E`

부동 소수점 숫자를 지수 표기법(Exponential notation, scientific notation)으로 치환한다.

```java
String.format("%e, %E", 20000.0f, 65536.888f); // "2.000000e+04, 6.553689E+04"
```

#### `f`

부동 소수점 숫자를 소수점 표기법으로 치환한다.

```java
String.format("%f, %f", 20000.0f, 65536.256f); // "20000.000000, 65536.257813"
```

#### `g` `G`

부동 소수점 숫자를 일반 형식으로 치환한다. 일정 자릿수 이하는 반올림하며(기준은 몲) 소수점 100만이 넘는 숫자는 지수 표기법을 쓴다.

```java
String.format("%g, %g, %g, %g, %G", 20.232f, 123.456f, 999999.0f, 1000000.0f, 1000020.0f);
// "20.2320, 123.456, 999999, 1.00000e+06, 1.00002E+06"
```

#### `a` `A`

부동 소수점 숫자를 16진수 부동 소수점 표기법으로 치환한다.

```java
String.format("%a, %a, %a, %a, %A", 20.232f, 123.456f, 999999.0f, 1000000.0f, 1000020.0f);
// "0x1.43b646p4, 0x1.edd2f2p6, 0x1.e847ep19, 0x1.e848p19, 0X1.E84A8P19"
```

#### `%`

퍼센트 기호 `%` 자체를 출력한다.

#### `n`

새 줄 문자, 그러니께 줄을 바꿀 때 쓴다.

#### `t` `T`

날짜와 시간을 포맷할 때 사용한다. `%t` 다음에 오는 문자로 어떤 포맷인지 지정할 수 있다.

시간 포맷 지시자:

- `H`: 두 자릿수의 하루 중의 시각을 24시간 형식으로 표시 (00 - 23)
- `I`: 두 자릿수의 하루 중의 시각을 12시간 형식으로 표시 (01 - 12)
- `k`: 하루 중의 시각을 24시간 형식으로 표시 (0 - 23)
- `l`: 하루 중의 시각을 12시간 형식으로 표시 (1 - 12)
- `M`: 1시간 중의 분을 두 자릿수로 표시 (00 - 59)
- `S`: 1분 중의 초를 두 자릿수로 표시 (00 - 60, 여기서 60은 윤초 지원용)
- `L`: 1초 중의 밀리초를 세 자릿수로 표시 (000 - 999)
- `N`: 1초 중의 나노초를 아홉 자릿수로 표시 (000000000 - 999999999)
- `p`: 지역 언어에 맞춘 오전/오후 표시. e.g., `am`/`pm`. 만약 `%Tp`면 대문자로 표시함
- `z`: GMT로부터 RFC 822 스타일 숫자 형식의 시간대 오프셋을 표시한다. e.g., `-0800`. 이 값은 필요한 경우 섬머타임을 적용하며, `long`, `Long`, `Date`인 경우 JVM의 기본 시간대를 사용함.
- `Z`: `z`와 같지만, 오프셋 대신 시간대 약어로 표시한다. e.g., `KST`
- `s`: 1970년 1월 1일 00:00:00(UTC)부터 초 단위의 시간 (`Long.MIN_VALUE / 1000` - `Long.MAX_VALUE / 1000`)
- `Q`: 1970년 1월 1일 00:00:00(UTC)부터 밀리초 단위의 시간 (`Long.MIN_VALUE` - `Long.MAX_VALUE`)

```java
LocalDateTime now = LocalDateTime.of(2023, 12, 17, 14, 53, 46, 123);
String.format("%1$tp %1$tH시 %1$tM분 %1$tS초", now); // "오후 14시 53분 46초"

// z, Z, s, Q는 어째서인지 java.time.LocalDateTime을 사용할 수 없어서 java.util.Date로 변환함 (LocalDateTIme에 지역값이 없나보다)
Date now2 = Date.from(now.toInstant(ZoneOffset.ofHours(9)));
String.format("%1$tz, %1$tZ, %tQ", now2); "+0900, KST, 1702792426000"
```

날짜 포맷 지시자:

- `B`: 지역 언어별 월의 전체 이름, e.g., January, February, 3월, ...
- `b` `h`: 지역 언어별 월의 짧은 이름, e.g., Jan, Feb, 3월, ...
- `A`: 지역 언어별 요일의 전체 이름, e.g., Sunday, Monday, 화요일, ...
- `a`: 지역 언어별 요일의 짧은 이름, e.g., Sun, Mon, 화, ...
- `C`: 세기(century)를 의미함. 4자리의 연도를 100으로 나누고 두 자릿수로 표시 (00 - 99)
- `Y`: 4자리로 표시한 연도 (0000 - 9999)
- `y`: 연도의 마지막 두 자릿수로 표시 (00 - 99)
- `j`: 1년 중의 일을 세 자릿수로 표시 (001 - 366)
- `m`: 1년 중의 월을 두 자릿수로 표시 (01 - 13, 13은 윤달인가? 🤔)
- `d`: 한 달 중의 일을 두 자릿수로 표시 (01 - 31)
- `e`: 한 달 중의 일을 표시 (1 - 31)

```java
String.format("%1$tB, %1$tb, %1$th", now); // "12월, 12월, 12월", 한국은 셋 다 똑같음 🤣
String.format("%1$tA, %1$ta", now); // "일요일, 일");
String.format("%1$tC, %1$tY, %1$ty", now); // "20, 2023, 23"
String.format("%1$tm월 %1$td일 (%1$te일, %1$tj일)", now); // "12월 17일 (17일, 351일)"
```

날짜와 시간 혹은 시간과 시간을 조합한 포맷 지시자:

- `R`: 24시간 형식으로 시간 표시(시분). `%tH:%tM`와 같음
- `T`: 24시간 형식으로 시간 표시(시분초). `%tH:%tM:%tS`와 같음
- `r`: 12시간 형식으로 시간을 표시. `%tI:%tM:%tS %Tp`와 같음.
- `D`: 날짜 표시. `%tm/%td/%ty`와 같음
- `F`: ISO 8601 형식으로 날짜 표시. `%tY-%tm-%td`와 같음
- `c`: 날짜와 시간을 표시. `%ta %tb %td %tT %tZ %tY`와 같음 e.g., `Sun Jul 20 16:17:00 EDT 1969`

```java
String.format("%1$tR", now); // "14:53"
String.format("%1$tT", now); // "14:53:46"
String.format("%1$tr", now); // "02:53:46 오후"
String.format("%1$tD", now); // "12/17/23"
String.format("%1$tF", now); // "2023-12-17"
String.format("%1$tc", now2); // "일 12월 17 14:53:46 KST 2023", 이것도 java.time.LocalDateTime을 쓸 수 없음
```

끗.
