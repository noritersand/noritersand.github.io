---
layout: post
date: 2013-08-16 05:00:00 +0900
title: '[Servlet] com.oreilly.servlet.MultipartRequest 파일 업로드 API'
categories:
  - servlet
tags:
  - java
  - servlet
  - multipartrequest
  - oreilly
  - file
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://servlets.com/cos/](http://servlets.com/cos/)
- [http://www.servlets.com/cos/javadoc/com/oreilly/servlet/MultipartRequest.html](http://www.servlets.com/cos/javadoc/com/oreilly/servlet/MultipartRequest.html)
- [http://noritersand.tistory.com/257](http://noritersand.tistory.com/257)
- [https://mvnrepository.com/artifact/servlets.com/cos/05Nov2002](https://mvnrepository.com/artifact/servlets.com/cos/05Nov2002)
- [cos-26Dec2008.zip](/attachments/cos-26Dec2008.zip)

Java Servlet 파일 업로드 API인 `MultipartRequest` 사용법을 간단히 기술한다.

## MultipartRequest

```
new MultipartRequest(HttpServletRequest request,
        String saveDirectory[, int maxPostSize, String encoding, FileRenamePolicy policy])
```

- `saveDirectory`: 파일 저장 경로
- `maxPostSize`: 파일의 최대크기, 최대크기를 초과하면 `IOException` 발생
- `encoding`: the encoding of the response, such as ISO-8859-1(왜 여기에 응답 인코딩을 지정하는지 모르겠다...)
- `policy`: 지정한 `savaDirectory` 내에 같은 이름을 가진 파일이 존재할 경우 덮어쓰기 여부, 명시하지 않으면 덮어쓴다.

MultipartRequest의 생성자는 request body 데이터를 읽어 지정한 경로에 파일을 생성한다. 생성자 호출과 동시에 파일이 생성되는 특징이 있어서 클라이언트가 전송한 파일의 선택적인 생성은 구현할 수 없다.

```html
<form method="post" action="/upload-test.do" enctype="multipart/form-data">
    제목: <input type="text" name="title" placeholder="제목을 입력하세요"><br>
    파일1: <input type="file" name="filename1"><br>
    파일2: <input type="file" name="filename2"><br>
    <input type="submit" value="업로드">
</form>
```

```java
import java.io.File;
import java.io.IOException;
import java.util.Arrays;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.oreilly.servlet.MultipartRequest;
import com.oreilly.servlet.multipart.DefaultFileRenamePolicy;

public class BoardServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        process(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        process(req, resp);
    }

    protected void process(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        String root = req.getSession().getServletContext().getRealPath("/");
        String pathname = root + "uploadDirectory";

        File f = new File(pathname);
        if (!f.exists()) {
            // 폴더가 존재하지 않으면 폴더 생성
            f.mkdirs();
        }

        String encType = "UTF-8";
        int maxFilesize = 5 * 1024 * 1024;

        // MultipartRequest(request, 저장경로[, 최대허용크기, 인코딩케릭터셋, 동일한 파일명 보호 여부])
        MultipartRequest mr = new MultipartRequest(req, pathname, maxFilesize,
                encType, new DefaultFileRenamePolicy());

        File file1 = mr.getFile("filename1");
        File file2 = mr.getFile("filename2");
        System.out.println(file1); // 첨부된 파일의 전체경로
        System.out.println(file2);

        System.out.println(req.getParameter("title")); // null
        System.out.println(mr.getParameter("title")); // 입력된 문자
    }
}
```

폼 태그에서 파일 업로드를 위해 `enctype` 속성을 `multipart/form-data`로 설정할 경우 해당 폼을 통해 전송된 데이터(이런 데이터는 요청 헤더가 아닌 바디를 통해 전송된다고 하여 request body post data라고 한다.)는 일반적인 방법인 `HttpServletRequest`의 파라미터로 접근할 수 없다. 실제로 위 소스를 실행해보면 `HttpServletRequest`의 파라미터는 `null`이며 대신 `MultipartRequest`의 파라미터로 가져오는 것을 확인할 수 있을 것이다.

참고로 위 소스를 이클립스-톰캣으로 돌렸을 때 파일이 저장되는 경로는 다음과 같은데:

```
...\[workspace]\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\[current project]\uploadDirectory
```

이것은 이클립스 톰캣 에드온으로 실행된 웹서버라서 이클립스에서 설정한 퍼블리싱 경로를 따른 결과다.
