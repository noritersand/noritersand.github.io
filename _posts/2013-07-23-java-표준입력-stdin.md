---
layout: post
date: 2013-07-23 22:13:00 +0900
title: '[Java] 표준입력 STDIN'
categories:
  - java
tags:
  - java
  - standard-input
  - stdin
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [아스키 코드표](https://ko.wikipedia.org/wiki/%EB%AF%B8%EA%B5%AD%EC%A0%95%EB%B3%B4%EA%B5%90%ED%99%98%ED%91%9C%EC%A4%80%EB%B6%80%ED%98%B8)

## System.in.read()

시스템으로부터 1바이트를 입력받아 정수형으로 리턴한다. 한글은 1바이트로 표현할 수 없기 때문에 숫자와 영문, 특수문자만 입력할 수 있다.

```java
int input = System.in.read(); // 1을 입력하면 케릭터 '1'의 아스키코드인 49가 저장됨.
System.out.println(input); // 49

char ch = (char) input; // 아스키코드 49가 '1'로 바뀜
System.out.println(ch); // '1'
```

`System.in.read()`를 사용하면서 주의할 점은 버퍼다. 콘솔에서 1을 입력했다고 해서 실제로 1만 입력되는 것이 아니라는 것:

```
1입력 → 1 + <LINE BREAK>
```

위처럼 `<LINE BREAK>`가 버퍼에 남아버려서 다음과 같은 현상이 발생한다:

```java
// OS가 윈도우일 때
System.in.read(); //입력대기
System.in.read(); //\r
System.in.read(); //\n
System.in.read(); //입력대기
```

따라서 콘솔에서 연속으로 입력을 받아야 하는 경우엔 InputStream의 `skip()` 메서드를 활용한다:

```java
System.in.read();
System.in.skip(2);
System.in.read();
```

## BufferedReader

문자, 배열, 행을 버퍼에 담은 후 문자형 입력 스트림으로 텍스트를 읽어 들인다.

```java
// read() : 1바이트를 읽어온다. 따라서 연속으로 사용하려면 버퍼초기화가 필요함.
// 이 함수는 System.in.read()와 작동 방식, 특징이 같음
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

br.read(); //입력대기
br.read(); //\r
br.read(); //\n
br.read(); //입력대기
```

BufferedReader 또한 버퍼를 염두에 두고 사용해야 한다. `read()` 메서드는 사용자로부터 입력받은 입력버퍼를 한 문자씩만 반환한다. 버퍼가 비어있으면 새로 요구하고, 버퍼가 남아있으면 요구없이 스스로 다음 문자를 반환한다. 즉, 3문자를 입력하면 `read()`를 다시 호출했을 때 입력값을 요청하는게 아닌 이미 입력된 문자 즉, 버퍼에서 값을 가져온다.

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

System.out.println(br.read());
System.out.println(br.read());
System.out.println(br.read());

// 여기서 123을 입력하면
-> 49
-> 50
-> 51
```

`readLine()`을 사용하면 버퍼초기화를 고려하지 않아도 되서 속편하다:

```java
System.out.println(br.readLine());  // 입력대기
System.out.println(br.readLine());  // 입력대기
```

## Scanner

```java
Scanner scan = new Scanner(System.in);
String str1 = scan.next();
String str2 = scan.next();
String str3 = scan.next();

// 여기서 a b c 를 입력하면
// 첫번째 next()는 a를,
// 두번째 next()는 b를,
// 세번째 next()는 c를 읽어옴
```

#### Scanner.next()

입력된 값 중 첫 단어만 읽어오며 나머지는 버퍼에 저장

#### Scanner.nextLine()

문자열(한 줄)을 읽는다. BufferedReader의 readLine()과 동일하다.

`nextInt()`, `nextDouble()`, 등 숫자형 데이터를 처리하는 메서드는 문자열을 읽어 해당 데이터형으로 변환한다. `next()` 처럼 공백을 기준으로 첫 어절만 읽어오며 나머지는 버퍼로 처리한다.

숫자입력과 문자입력을 격리된 지역에서 각각 사용한다면 상관없지만 숫자입력 직 후 같은 지역 내에서 문자입력을 받는다면 버퍼문제가 발생한다.

앞서 말했듯이 `next()`의 파생형인 `nextInt()` 등의 메서드는 어절 단위로 읽어오기 때문인데 만약 다음처럼:

```java
int number = scn.nextInt();  // 여기서 123을 입력했다 가정
```

일 경우엔 버퍼에 <엔터>가 남아있는 상태다.

여기서 다시:

```java
String str = scn.nextLine();
```

처럼 작성하면 `nextLine()`은 콘솔 입력을 기다리는게 아니라 일단 버퍼에 있는 값을 가져오게 된다. 따라서 숫자형 입력을 받은 직후 다시 문자를 입력받아야 한다면 반드시 다음처럼 버퍼 초기화를 고려해야 한다:

```java
int number = scn.nextInt();
scn.nextLine();   // 버퍼 초기화
String str = scn.nextLine();
```
