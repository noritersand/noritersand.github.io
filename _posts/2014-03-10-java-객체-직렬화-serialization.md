---
layout: post
date: 2014-03-10 18:25:00 +0900
title: '[Java] 객체 직렬화 serialization'
categories:
  - java
tags:
  - java
  - serialization
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [Java 객체 직렬화에 대해 당신이 모르고 있던 5가지](/attachments/j-5things.pdf)


## 직렬화란?

객체에 저장된 데이터를 스트림에 쓰기 위해 연속적인 데이터로 변환하는 것을 말한다. 반대로 스트림에서 데이터를 읽어 객체로 변환하는 것을 역직렬화(deserialization)라 한다.


## 직렬화, serialize

```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class JavaSerializationSample {
    public static void main(String[] args) throws IOException {

        VO vo = new VO();
        vo.setName("호옹이");
        vo.setAge(11);
        vo.setEmail("a@a.com");

        FileOutputStream fos = new FileOutputStream("c:\\iotest\\serial.txt",
                false);
        BufferedOutputStream bos = new BufferedOutputStream(fos);
        ObjectOutputStream out = new ObjectOutputStream(bos);

        out.writeObject(vo);
        out.close();
    }
}

class VO implements Serializable {
    private static final long serialVersionUID = -395246598280398285L;

    private String name;
    private int age;
    private String email;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

인스턴스를 직렬화하여 파일로 내보내는 코드다. 이처럼 자바 직렬화는 ObjectOutputStream의 `writeObject()`를 이용해 아주 간단하게 구현할 수 있다.(BufferedOutputStream은 반드시 거치지 않아도 되는 보조스트림)

단, 직렬화 대상이 되는 클래스는 반드시 Serializable 인터페이스를 구체화(implements)해야 하는데, 그렇지 않으면 NotSerializableException이 발생한다.

```java
import java.io.Serializable;

public class VO implements Serializable{
    String name; // 직렬화 가능
    String email; // 직렬화 가능
    int age; // 직렬화 가능
    Object tell = new Object(); // Object의 인스턴는 직렬화 할 수 없음.
    Object tell2 = new String("111-111-111"); // 실제 인스턴스는 String이므로 직렬화 가능

// 생략
```

또한 Object 클래스의 인스턴스는 직렬화 할 수 없다. 모든 클래스의 조상인 Object 클래스는 Serializable을 구체화하기 않았기 때문인데, 실제 인스턴스가 Serializable을 구체화한 클래스의 인스턴스이며 Object 타입으로 업캐스팅된 경우라면 직렬화가 가능하다. (가령 String을 Object로 업캐스팅한 경우)

```java
class Parent {
    String name; // 직렬화 불가
}

class Child extends Parent implements Serializable {
    String password; // 직렬화 가능
}
```


## 역직렬화, deserialize

역직렬화는 ObjectInputStream의 `readObject()`를 이용한다. 다만 한 가지 주의 사항이 있는데 하나 이상의 객체를 직렬화 했을 경우 반드시 같은 순서로 역직렬화 해야 한다는 것이다.

예를 들어:

```java
FileOutputStream fos = new FileOutputStream("c:\\iotest\\serial.bin", false);
BufferedOutputStream bos = new BufferedOutputStream(fos);
ObjectOutputStream out = new ObjectOutputStream(bos);

VO vo1 = new VO("ob1", "aaa@domain.com", 11);
VO vo2 = new VO("ob2", "bbb@domain.com", 12);
ArrayList<VO> list = new ArrayList<>();
list.add(vo1);
list.add(vo2);

out.writeObject(vo1); // first object
out.writeObject(vo2); // second object
out.writeObject(list); // third object

out.close();
```

위와 같을 때 다음처럼 직렬화와 같은 순서로 역직렬화한다:

```java
FileInputStream fis = new FileInputStream("c:\\iotest\\serial.txt");
BufferedInputStream bis = new BufferedInputStream(fis);
ObjectInputStream ois = new ObjectInputStream(bis);

VO vo1 = (VO) ois.readObject(); // first object
VO vo2 = (VO) ois.readObject(); // second object
ArrayList<VO> list = (ArrayList<VO>) ois.readObject(); // third object

System.out.println(vo1);
System.out.println(vo2);
System.out.println(list.get(0));
System.out.println(list.get(1));

ois.close();
```


## transient

선언부에 `transient` 제어자를 사용하면 직렬화에서 제외된다.

```java
public class VO implements Serializable{
    int age;
    String name;
    transient String email; // 직렬화 대상에서 제외되며 역직렬화 시 기본값으로 초기화된다.
}
```


## serialVersionUID

`serialVersionUID`란 역직렬화 시 클래스간 버전 비교에 사용되는 해시값이다.

명시하지 않을 경우 객체가 직렬화될 때 클래스에 정의된 멤버들의 정보를 이용해서 `serialVersionUID`에 클래스의 버전을 JVM이 자동으로 생성하여 직렬화 내용에 포함한다.

```java
public class VO implements Serializable {
    private static final long serialVersionUID = 8987889780557008769L;

// 생략
```

만약 직렬화 시점의 serial version과 역직렬화 시점의 serial version이 일치하지 않는다면 아래처럼 `java.io.InvalidClassException`이 발생하면서 역직렬화는 실패한다.

```java
Exception in thread "main" java.io.InvalidClassException: com.test.VO;
local class incompatible: stream classdesc serialVersionUID = 1001, local class serialVersionUID = 1002
```


## 직렬화된 데이터의 암호화

`javax.crypto.SealedObject` wrapper 클래스나 `java.security.SignedObject` wrapper 클래스를 이용하면 데이터를 봉인하거나 암호화할 수 있다. 자세한 내용은 아래 링크를 참고할 것.

[Java 객체 직렬화에 대해 당신이 모르고 있던 5가지](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwj02-n5ppffAhUBWbwKHUq9CgEQFjAAegQIBxAC&url=http%3A%2F%2Fcfile30.uf.tistory.com%2Fattach%2F26613D375537C8D71B6149&usg=AOvVaw0d13LGC4OGxmQj2UCEE2jC)
