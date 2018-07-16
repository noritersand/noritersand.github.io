---
title: 'Java: 특정 폴더의 모든 파일명 변경'
categories:
  - 코드모음
date: 2018-07-16 09:12:51
tags:
  - java
  - lambda
  - JDK8
  - stream
---

#### required
- JDK8 or higher

```java
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;
import java.util.stream.Stream;

public class ReplaceFileName {
	private static final String targetLocation = "c:/temp";
	private static final String prefix = "02-alarm-";
	private static final String suffix = "";
	private static final String destLocation = "c:/dest";

	public static void main(String[] args) throws Exception {
		Path path = new File(targetLocation).toPath();
		Stream<Path> list = Files.list(path);

		// list.forEach(System.out::println);

		list.forEach((k) -> {
			final String fileName = k.toString();
			final String newFileName = prefix
					+ fileName.substring(fileName.lastIndexOf(File.separator) + 1, fileName.length())
					+ suffix;
			Path newPath = new File(destLocation, newFileName).toPath();
			try {
				Files.move(k, newPath, StandardCopyOption.REPLACE_EXISTING);
			} catch (IOException e) {
				System.err.println(e);
			}
		});

		list.close();
	}
}
```
