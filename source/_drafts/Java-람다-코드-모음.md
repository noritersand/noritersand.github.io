---
title: 'Java: 람다 코드 모음'
tags:
---

## 파일명 일괄 변경하기

```java
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;
import java.util.stream.Stream;

public class ReplaceFileName {
	public static void main(String[] args) throws Exception {
		Path path = new File("c:/temp").toPath();
		Stream<Path> list = Files.list(path);

		// list.forEach(System.out::println);

		list.forEach((k) -> {
			final String fileName = k.toString();
			final String newFileName = "02-alarm-"
					+ fileName.substring(fileName.lastIndexOf("\\") + 1, fileName.length());
			Path newPath = new File("c:\\temp\\" + newFileName).toPath();
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
