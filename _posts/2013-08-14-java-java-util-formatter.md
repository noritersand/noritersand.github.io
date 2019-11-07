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

## format()

```java
format( [Locale l,] String format, Object... args )
```

```java
"%[argument_index$][flags][width][.precision]conversion"
```

- `l`: 서식 설정 시에 적용하는 locale(국가별 설정). l 이 null 의 경우 무시
- `format`: 출력할 자료의 서식
- `%:` 제어 문자 시작
- `argument_index`: 인수 리스트 내에서의 인수의 위치를 나타내는 10진수로 최초의 인수는 `1$`, 2 번째의 인수는 `2$`로 표시한다.
- `flags`: 출력 서식을 변경하는 문자세트
- `-:` 왼쪽 끝에 출력 (default: 오른쪽 정렬)
- `width`: 필드 폭
- `precision`: 실수의 소수점이하 자릿수로 `g`(`G`) 변환문자의 경우에는 결과의 자리수이며, `a`(`A`) 의 변환문자는 precision 는 지정되지 않는다. 또한   문자열은 가능하나 문자, 정수, 일자/시 각 인수 타입, 및 퍼센트 등에서는 precision을 적용할 수(실수의 디폴트는 6이며 0인 경우 1로 간 주)
- `conversion`: 변환문자

```java
String result = str.Stringformat("입력하신 값은: %d입니다.", 30);
```

## 출력 형식 지정 제어문자 표

| 기호 | 설명 |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `+` |  양의 정수의 출력에서 부호(+)를 표시한다. |
| `#` |  `o`, `x`, `X` 변환 문자에서만 사용되며 `o` 변환 문자와 함께 사용하는 경우 앞에 `0`을 표시하 고, `x` 변환 문자와 함께 사용하는 경우 앞에 `0x`(`0X`)를 표시 8진수 또는 16진수임을 표시 한다. |
| `0` |  width와 함께 사용되며 남은자리에 공백 대신 `0`으로 표시(정수, 실수형). 세 자리마다 컴마 표시(`d`, `e`, `E`, `f`, `g`, `G` 함께 사용) |
| `(` |  양수인 경우에는 무시되며 음수인 경우 `()`에 둘러싸여 표시 |
| `b`, `B` |  boolean 또는 Boolean 값을 `String.valueOf()`에 의해 표시하고, arg가 null인 경우 결과는 false로 표시하며 그렇지 않으면 true |
| `h`, `H` |  16진수(`Integer.toHexString(arg.hashCode())`)로 표시하고, arg가 null인 경우 null로 표시 |
| `s`, `S` |  문자열을 출력(`arg.toString()`)하며 arg가 null인 경우 null로 표시되고, arg가 Formattable을 구현하는 경우에는 arg.formatTo 메서드를 호출 한다. `c`, `C`: Unicode 문자 |
| `d`, `o` |  10진수(decimal), 8진수(octonary) |
| `x`, `X` |  16진수(hexadecimal) |
| `e`, `E` |  실수를 지수로 표시(`m.nnnnnne[±]xx`) |
| `f` |  실수를 표시(`[±]m.nnnnnn`) |
| `g`, `G` |  `%f` 나 `%e`중 자리수가 적은 쪽으로 변환 표시 a, A: 실수를 16진수로 출력 |
| `t`, `T` |  일자 및 시각 변환 문자용의 접두사 |
| `tA` |  요일, `tY`: 4자리 연도, `tB`: 월 이름, `tm`: 01~12로 표현되는 월 |
| `te` |  1~31 까지 의 해당 월 날짜, `tk`: 0 ~ 23 으로 표현되는 시 |
| `tl` |  1 ~ 12로 표현되는 시, `tM`: 00~59로 표현되는 분, `tS`: 00~59로 표현되는 초, `tZ`: 타임 존 |
| `%` |  `%(\u0025)` 자체를 출력 `n`: 라인을 넘김  |

### 예시

- `%10d`, `%-10d`: 고정출력폭, 여백, -의 경우 오른쪽여백
- `%.5f`, `%.1f`: 지정한 길이를 초과한 소숫점의 값을 반올림
- `%6.1f`: 6칸의 여백을 확보한 뒤 소숫점 1자리까지 출력
- `%,d`: 1000자리마다 쉼표 출력
- `%,2.1f`: 응용
- `% [인자 번호] [플래그] [너비] [.정밀도]`: 유형
- `%d`: 십진정수, `%f`: 부동소수점, `%x`: 16진수, `%c`: 문자
- `%tc`: 날짜와 시간 전부 표시
- `%tr`: 시간만 표시할때
- `%A %B %C`: 요일,월,일 표시

이 외에는 API 문서를 확인할 것

## example

```java
YYYY-MM-DD:

Calendar cal = Calendar.getInstance();
System.out.println("오늘 : " + String.format("%tF", cal));

cal.add(Calendar.MONTH, -3);
System.out.println("세달전 : " + String.format("%tF", cal));

String s = String.format("%, d", 1000000000);
System.out.println(s);   // I have 44444.44 bugs to fix

String fo = String.format("I have %.2f bugs to fix.", 44444.444);
System.out.println(fo);   // I have 2,231,323.23 bugs to fix.

String fo1 = String.format("I have %,.2f bugs to fix ", 2231323.23132);
System.out.println(fo1);   // 1,323,131,123.1

String fo2 = String.format("%,6.1f", 1323131123.133123213);
System.out.println(fo2);   // 금 9월 05 10:55:14 KST 2008

String fo3 = String.format("%tc", new Date() );
System.out.println(fo3);   // 10:55:14 오전

String fo4 = String.format("%tr", new Date() );
System.out.println(fo4);   // 금요일 9월 20

String fo5 = String.format("%tA %tB %tC", new Date(), new Date(), new Date() );
System.out.println(fo5);   // 금요일 9월 20
```
