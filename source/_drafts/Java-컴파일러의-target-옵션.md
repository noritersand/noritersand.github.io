---
title: Java 컴파일러의 target 옵션
categories:
  - java
tags:
  - java
  - javac
  - compiler
  - todo
---

자바 컴파일러인 javac는 target 옵션이 존재한다. 이 옵션은 특정 Java 버전(보통은 현재 사용중인 JDK보다 낮은)에서도 실행가능한 클래스 파일을 만드는 옵션이다.
```bash
javac -target 8 Test.java
# Test.java 파일을 컴파일하되 Java 8에서 실행 가능하도록 호환처리한다.
```

이클립스에서는 이 옵션이 `Window` > `Preferences` 혹은 각 프로젝트의 `Properties` 설정화면에 `Java Compiler` 메뉴로 제공된다.
![](/images/java-컴파일러의-target-옵션-image-1.png)

메이븐에선 pom.xml에서 컴파일러 플러그인 설정으로 target을 지정할 수 있다.
```xml
<build>
  <finalName>laboratory</finalName>
  <plugins>
    <plugin>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.5.1</version>
      <configuration>
        <source>${java-version}</source>
        <target>${java-version}</target>
        <encoding>${encoding}</encoding>
      </configuration>
    </plugin>
  </plugins>
</build>
```

사족으로, 이클립스는 컴파일 에러가 나는 자바파일도 일단 class 파일로 만든다. 이게 이클립스만 그러는건지는 아직 확실하지 않은데, 일단은 메서드 단위로 컴파일한다는게 확인되었다.
```java
public class Test {
  public void m01() {
    System.out.println("hi");
  }

  public void m02() {
    throw new Error("Unresolved compilation problem: \n\tabc cannot be resolved to a variable\n");
  }
}
```
요딴식으로.
