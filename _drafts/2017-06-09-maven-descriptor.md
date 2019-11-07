---
layout: post
date: 2017-06-09 19:12:00 +0900
title: '[Maven] descriptor'
categories:
  - maven
tags:
  - buildtool
  - maven
  - descriptor
---

* Kramdown table of contents
{:toc .toc}

이거 뭐냐

#### pom.xml

```xml
<plugin>
    <!-- assamble static content -->
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
        <descriptors>
            <descriptor>src/main/resources/static.xml</descriptor>
        </descriptors>
    </configuration>
    <executions>
        <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

#### static.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<assembly>
    <id>static</id>
    <formats>
        <format>zip</format>
    </formats>
    <includeBaseDirectory>false</includeBaseDirectory>
    <fileSets>
        <fileSet>
            <directory>src/main/webapp</directory>
            <includes>
                <include>/api/**/*</include>
                <include>/css/**/*</include>
                <include>/error/**/*</include>
                <include>/guide/**/*</include>
                <include>/img/**/*</include>
                <include>/js/**/*</include>
                <include>/pub/**/*</include>
                <include>/template/**/*</include>
            </includes>
            <excludes>
                <exclude>WEB-INF/*</exclude>
            </excludes>
            <outputDirectory></outputDirectory>
        </fileSet>
    </fileSets>
</assembly>
```

뭐하는놈인거시냐
