---
layout: post
date: 2013-07-23 22:17:00 +0900
title: '[Java] Input/Output Stream'
categories:
  - java
tags:
  - java
  - stream
  - writer
  - reader
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [아스키 코드표](https://ko.wikipedia.org/wiki/%EB%AF%B8%EA%B5%AD%EC%A0%95%EB%B3%B4%EA%B5%90%ED%99%98%ED%91%9C%EC%A4%80%EB%B6%80%ED%98%B8)

이 글은 남궁성 저 'Java의 정석'을 참고하여 작성함.  http://cafe.naver.com/javachobostudy.cafe

#### 인코딩, encoding이란?

응용프로그램의 데이터를 스트림(Stream)형식으로 변환시켜 보조기억장치나 네트워크상에서 사용가능한 형태로 변환하는 작업.

#### 스트림, stream이란?

일련의 연속된 데이터의 흐름으로 자바프로그램과 외부장치 사이의 데이터 교환을 위한 처리 방식이다. 추상화, 실제 장치와 상관없이 공통된 접근 방식을 제공한다.

## 바이트 기반 스트림: InputStream, OutputStream

바이트 기반 스트림은 바이트단위로 데이터를 전송하는 클래스로 InputStream과 OutputStream을 상속받는 FileStream, ByteArrayStream, PipedStream, AudioStream, StringBufferStream 등이 있다.

바이트 기반 스트림은 종류에 따라 `mark()`와 `reset()`을 사용해 특정 지점을 마킹하고 그 지점에서 다시 읽는것이 가능하다. `flush()`는 버퍼가 있는 출력용 스트림(OutputStream을 확장한 클래스들)의 경우에만 의미가 있고, OutputStream에 정의된 `flush()`는 아무것도 하지 않는다.

### FileIntputstream/FileOutputStream

파일 입출력에 사용되는 스트림 클래스.

```java
File file = new File("c:\\iotest", "stream.txt");
if (! file.exists()) {
    file.createNewFile();
}

// 쓰기
FileOutputStream fos = new FileOutputStream("c:\\iotest\\stream.txt", true);
// true : Append, 이어쓰기
// false : Create, 기존 내용에 덮어쓰기
// 기본값 : false

fos.write(97); // a가 stream.txt에 쓰여짐
fos.write(98); // b가 stream.txt에 쓰여짐
fos.write(99); // c가 stream.txt에 쓰여짐
fos.close();

// 읽기
FileInputStream fis = new FileInputStream("c:\\iotest\\stream.txt");

int data = 0;
while ((data = fis.read()) != -1) { // 더 이상 읽을것이 없을 때 -1
    System.out.print((char) data); //스트림이 읽은 바이트코드를 문자로 변환하여 출력
}
fis.close();
```

아래는 파일을 읽어와 그대로 다른파일에 내용을 출력하는 코드다:

```java
FileInputStream fis = new FileInputStream("c:\\iotest\\origin.txt");
FileOutputStream fos = new FileOutputStream("c:\\iotest\\clone.txt", false);         

int data = 0;
while ((data = fis.read()) != -1) { // 더 이상 읽을것이 없을 때 -1
    fos.write(data);
}
fis.close();
fos.close();
```

다음처럼 작성하면 String 변수의 값을 출력한다:

```java
FileOutputStream fos = new FileOutputStream("c:\\iotest\\stream.txt", false);

String txt = "안녕! Fine and strong day! im waldo!";

for (int i = 0; i < txt.length(); i++) { //문자열의 길이만큼
    char c = txt.charAt(i); // 문자로 형변환 후
    fos.write((int) c); // 출력
}
fos.close();
```

단, 위 방법은 한글을 처리할 수 없다. 바이트 스트림으로 한글을 처리하고 싶다면 다음처럼 한다:

```java
FileOutputStream fos = new FileOutputStream("c:\\iotest\\stream.txt", false);
String txt = "안녕! Fine and strong day! im waldo!";
fos.write(txt.getBytes());
fos.close();
```

## 바이트 기반 보조 스트림

스트림의 기능을 보완하기 위한 클래스를 보조 스트림이라 하며 바이트기반, 문자기반 스트림 모두 보조 스트림이 제공된다.

보조 스트림은 실제 데이터를 주고 받는 스트림이 아니기 때문에 보조 스트림만으로는 입출력을 처리할 수 없고, 바이트 기반 스트림 혹은 문자 기반 스트림을 먼저 생성한 다음 이를 이용해 보조 스트림을 생성한다.

```java
// 기반 스트림 생성
FileInputStream fis = new FileInputStream("c:\\iotest\\sample.txt");

// 기반 스트림을 이용한 보조 스트림 생성
BufferedInputStream bis = new BufferedInputStream(fis);
bis.read();
```

위를 보면 BufferedInputStream의 `read()`를 사용한 것 같지만 실제 입력기능은 기반 스트림인 FileInputStream이 수행하고 BufferedInputStream은 버퍼 기능만 제공한다.

이처럼 보조 스트림의 경우 대부분 입출력에 버퍼를 이용한다는 특징이 있는데 얼핏 별거 없어 보이지만 데이터의 양이 많을 수록 버퍼가 처리 속도에 상당한 영향을 끼친다.

### BufferedInputStream/BufferedOutputStream

버퍼를 이용해 스트림의 입출력 효율을 높이기 위해 제공되는 보조 스트림이다.

한 바이트 씩 읽고 쓰는 것보다 버퍼(바이트배열)을 이용해 여러 바이트를 한 번에 처리하는 것이 빠르기 때문에 대부분의 입출력 작업에 사용된다.

생성자에 버퍼 크기를 지정할 수 있는데 이를 생략하면 기본값으로 8192 byte의 버퍼를 갖게 된다. 버퍼 크기는 입력 소스로부터 한 번에 가져올 수 있는 데이터의 크기로 지정하면 좋다고 한다카더라.

```java
File file = new File("c:\\iotest\\sample.txt");

FileInputStream fis = new FileInputStream(file);
BufferedInputStream bis = new BufferedInputStream(fis, 4096); // 버퍼 사이즈를 지정할 수 있다.

while (bis.available() > 0) {
    System.out.print((char) bis.read());
}
bis.close();
```

여기서는 `available()`을 사용했는데 이는 Stream 클래스의 메서드로 스트림으로부터 읽어 올 수 있는 데이터의 크기를 리턴한다.

#### BufferedStream 사용 예

```java
// 파일 복사
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("C:/iotest/readme.txt"));
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("C:/iotest/writeme.txt"));

int len = 0;

while (bis.available() > 0) {
    len ++;
    bos.write(bis.read());
}

System.out.printf("총 %,d바이트 복사됨\n", len);

bos.flush();
bis.close();
bos.close();


// String 직접 쓰기
BufferedOutputStream bos2 = new BufferedOutputStream(new FileOutputStream("C:/iotest/result.txt"));
String txt = "안녕하신가?\r\ngood morning!";
bos2.write(txt.getBytes());
bos2.flush();
bos2.close();
```

### DataInputStream/DataOutputStream
FileterStream을 상속하면서 DataInput/DataOutput 인터페이스를 구현한 클래스. 모든 원시형 값을 읽고 쓸 수 있다.

DataOutputStream은 출력 시 각 원시형 값을 16진수로 표현하여 저장한다. 예를 들어 int형 값은 4byte의 16진수로 출력된다.

DataStream은 문자를 직접 저장하는 것보다 속도가 빠르다는 특징이 있다. 이는 data와 문자간 변환 과정이 없기 때문이다.

```java
File file = new File("c:\\iotest\\sample.dat");

// 쓰기
DataOutputStream dos = new DataOutputStream(new FileOutputStream(file));
dos.writeBoolean(true);
dos.write(5);
dos.writeByte((byte) 5);
dos.writeInt(100);
dos.writeDouble(200.5);
dos.writeUTF("utfutfutf");

dos.flush();
dos.close();

// 읽기
DataInputStream dis = new DataInputStream(new FileInputStream(file));
System.out.println(dis.readBoolean());
System.out.println(dis.read());
System.out.println(dis.readByte());
System.out.println(dis.readInt());
System.out.println(dis.readDouble());
System.out.println(dis.readUTF());
dis.close();
```

특정한 변환작업이나 자릿수를 셀 필요없이 읽기만 하면 되는데 주의할 점이 있다. 데이터를 쓰는 과정에서 자료형을 달리해서 저장할 경우 읽어올 때도 반드시 그 순서에 맞게 읽어와야 한다는 것이다. 만약 이 순서가 바뀌면 read 메서드들이 올바른 자료형으로 변환하지 못해 엉뚱한 값을 출력하게 된다.

## 문자 기반 스트림: Reader, Writer

바이트 기반 스트림은 1byte단위로 읽고 쓰기 때문에 크기가 2byte 이상인 문자를 다루는데 문제가 있다. 문자 기반 스트림은 이 점을 보완하기 위한 클래스다. 또한 문자 인코딩간 변환을 자동으로 처리해준다는 장점이 있다.

Reader와 Writer 클래스는 바이트 스트림에서 처럼 문자 기반 스트림 클래스들이 상속받은 부모 클래스로 byte 배열이 아닌 char 배열을 사용하는 `read()`, `write()`가 추상메서드로 구현되어 있다.

### FileReader/FileWriter

```java
// 쓰기
FileWriter out = new FileWriter("c:\\iotest\\sample.txt", true);
//true: Append, 이어쓰기
//false: Create, 기존 내용에 덮어쓰기
//기본값 : false

String txt = "You just activated \r\nMY TRAP CARD \r\n붑밥붑밥붑밥바";
out.write(txt);
out.close();

// 읽기
FileReader in = new FileReader("c:\\iotest\\sample.txt");
int data = 0;
while((data = in.read()) != -1) { // 파일 내용의 길이만큼 루프
    System.out.print((char) data); // 읽어온 데이터를 문자로 변환 - 출력
}
in.close();
```

```java
// 파일 복사
FileReader in = new FileReader("c:\\iotest\\sample.txt");
FileWriter out = new FileWriter("c:\\iotest\\clone.txt");

int data = 0;
while ((data = in.read()) != -1) {
    out.write(data);
}

in.close();
out.close();

// String 바로 쓰기
FileWriter out2 = new FileWriter("c:\\iotest\\result.txt");
String txt = "안녕? abcdefg\r\nhihi~";
out2.write(txt);
out2.close();
```

## 문자 기반 보조 스트림

### BufferedReader/BufferedWriter

버퍼를 이용해 Reader와 Writer의 입출력 효율을 개선한 클래스.

```java
// 쓰기
BufferedWriter out = new BufferedWriter(new FileWriter("c:\\iotest\\buffer.txt", false));
out.write("버퍼드리더를 활용한 \r\n파일출력");
out.flush();        
out.close();

// 읽기
BufferedReader in = new BufferedReader(new FileReader("c:\\iotest\\buffer.txt"));
String data;
while((data = in.readLine()) != null) { //읽을게 없으면 null 리턴
    System.out.println(data);
}
in.close();

//// read()를 사용할 경우
//int data;
//while((data = in.read()) != -1) {
//    System.out.print((char) data);
//}
//in.close();
```

바이트 기반 보조 스트림과 마찬가지로 `flush()`를 실행하거나 스트림을 닫아야 버퍼의 데이터가 출력된다.

`read()`는 stream의 경우와 사용방법은 거의 차이나지 않지만 새롭게 추가된 `readLine()`은 `read()`와 다르게 줄 단위로 데이터를 읽어오며 더 이상 읽을 데이터가 없으면 -1이 아니라 null을 리턴한다.

```java
// 파일 복사
BufferedReader in = new BufferedReader(new FileReader("c:\\iotest\\sample.txt"));
BufferedWriter out = new BufferedWriter(new FileWriter("c:\\iotest\\clone.txt"));
String data = "";
while ((data = in.readLine()) != null) {
    out.write(data + "\r\n");
}
out.flush();
in.close();
out.close();

// String 직접 쓰기
BufferedWriter out2 = new BufferedWriter(new FileWriter("c:\\iotest\\result.txt"));
String txt = "호옹이";
out2.write(txt);
out2.flush();
out2.close();
```

#### 응용 - 세미콜론";"을 포함한 줄만 출력

```java
// 읽기
BufferedReader in = new BufferedReader(new FileReader("c:\\iotest\\Test1.java"));

String data = "";
int cnt = 0;
while ((data = in.readLine()) != null) {
    cnt ++;
    if (data.indexOf(";") != -1) {
        System.out.println(cnt + ":" + data);
    }
}
```

### InputStreamReader/OutputStreamWriter

바이트 기반 스트림을 문자 기반 스트림으로 연결시켜주는 역할을 한다. 읽거나 쓸 때 문자 인코딩을 지정할 수 있다.

```java
File file = new File("c:\\iotest\\sample.txt");
InputStreamReader inStrReader = new InputStreamReader(new FileInputStream(file), "utf-8");
BufferedReader in = new BufferedReader(inStrReader);

String data = "";
while ((data = in.readLine()) != null) {
    System.out.println(data);
}
in.close();
```

터미널을 통해 입력받을 땐 보통 아래처럼 쓴다:

```
BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
//BufferedReader in = new BufferedReader(new InputStreamReader(System.in, "utf-8"));
```

## PrintWriter

PrintStream보다 향상된 문자기반 스트림이다. JDK1.1에 추가되었다.

```java
// PrintStream
System.out.println("hi"); //new PrintStream(System.out).println("hello world");

// PrintWriter
PrintWriter pw = new PrintWriter(System.out);
pw.write("hi");
pw.flush();
pw.close();
```
