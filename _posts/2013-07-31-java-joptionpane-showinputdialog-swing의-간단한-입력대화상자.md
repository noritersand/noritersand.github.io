---
layout: post
date: 2013-07-31 23:36:00 +0900
title: 'Java: JOptionPane.showInputDialog(swing의 간단한 입력대화상자)'
categories:
  - java
tags:
  - java
  - swing
  - dialog
---

* Kramdown table of contents
{:toc .toc}

```java
package com.test;

import javax.swing.JOptionPane;

public class LogicTest {
    public static void main(String[] args) throws Exception {

        String input = "";

        while (true) {
            input = JOptionPane.showInputDialog("하이");

            System.out.println("\"" + input + "\"" + "을 입력하였습니다.");

            if (input.equals("-1"))
                break;
        }
    }
}
```

![](/images/java-swing-dialog-1.png)

![](/images/java-swing-dialog-2.png)
