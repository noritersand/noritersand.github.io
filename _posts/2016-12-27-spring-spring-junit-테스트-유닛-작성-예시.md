---
layout: post
date: 2016-12-27 16:32:00 +0900
title: '[Spring] Spring-JUnit: 테스트 유닛 작성 예시'
categories:
  - spring
tags:
  - java
  - spring
  - junit
  - code-test
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

### SpEL-프로퍼티 접근 테스트

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p" xmlns:util="http://www.springframework.org/schema/util"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <util:properties id="sendMailInfo">
        <prop key="addressSupport">hi</prop>
    </util:properties>
</beans>
```

```java
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/propertyTest/*.xml")
//@ContextConfiguration(locations = "classpath:propertyTest/test-property-context.xml")
public class PropertyTest {
    @Value("#{sendMailInfo.addressSupport}")
    private String senderEmailAddress;

    @Test
    public void getPropertyTest() {
        Assert.assertEquals("hi", senderEmailAddress);
    }
}
```

### 인터셉터 테스트

```java
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mock.web.MockHttpServletRequest;
import org.springframework.mock.web.MockHttpServletResponse;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.web.servlet.HandlerExecutionChain;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter;
import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping;

@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration("/interceptorTest/dispatcher-servlet.xml")
public class InterceptorTest {
    @Autowired
    private RequestMappingHandlerAdapter handlerAdapter;

    @Autowired
    private RequestMappingHandlerMapping handlerMapping;

    @Test
    public void testInterceptor() throws Exception {
        MockHttpServletRequest request = new MockHttpServletRequest();
        request.setRequestURI("/test");
        request.setMethod("GET");

        MockHttpServletResponse response = new MockHttpServletResponse();

        HandlerExecutionChain handlerExecutionChain = handlerMapping.getHandler(request);

        HandlerInterceptor[] interceptors = handlerExecutionChain.getInterceptors();

        for (HandlerInterceptor interceptor : interceptors) {
            interceptor.preHandle(request, response, handlerExecutionChain.getHandler());
        }

        ModelAndView mav = handlerAdapter. handle(request, response, handlerExecutionChain.getHandler());

        for (HandlerInterceptor interceptor : interceptors) {
            interceptor.postHandle(request, response, handlerExecutionChain.getHandler(), mav);
        }

        //assert the success of your interceptor
        Assert.assertEquals(200, response.getStatus());
    }
}
```

### MockMultipartFile 파일 업로드 테스트

```java
import static org.junit.Assert.*;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Map;

import org.apache.commons.lang3.ArrayUtils;
import org.joda.time.DateTime;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;
import org.springframework.mock.web.MockMultipartFile;
import org.springframework.mock.web.MockMultipartHttpServletRequest;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.web.multipart.MultipartFile;

import com.etbs.common.io.filestorage.FileUploader;
import com.etbs.common.io.filestorage.UploadedFile;
import com.etbs.common.io.filestorage.policy.DefaultFileUploadPolicy;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/fileTest/*.xml")
public class FileUploaderTest {
    @Value("#{fileResourceMap}")
    private Map fileResourceMap;

    private String tempFilePath = "/upload/temp/example/helloworld.png";
    private String filePath = "/upload/helloworld.png";
    private String fileName = "/helloworld.png";

    @Autowired
    private FileUploader fileUploader;

    private Resource webRootResource;
    private Resource uploadResource;
    private Resource tempResource;
    private Resource realResource;

    private MockMultipartHttpServletRequest mreq;

    public void initialize() throws IOException {
        if (webRootResource == null) {
            webRootResource = fileResourceMap.get("webRoot");
            uploadResource = fileResourceMap.get("upload");
            tempResource = fileResourceMap.get("exampleImageTemp");
            realResource = fileResourceMap.get("exampleImage");
        }
        if (mreq == null) {
            Path path = getFile().toPath();
            byte[] data = Files.readAllBytes(path);
            MockMultipartFile file = new MockMultipartFile(fileName, fileName, "image/png", data);
            mreq = new MockMultipartHttpServletRequest();
            String boundary = "q1w2e3r4t5y6u7i8o9";
            mreq.setContentType("multipart/form-data;");
            mreq.setContentType("multipart/form-data; boundary=" + boundary);
            mreq.setContent(createFileContent(data, boundary, "image/png", fileName));
            mreq.addFile(file);
            mreq.setMethod("POST");
            mreq.setParameter("variant", "php");
            mreq.setParameter("os", "mac");
            mreq.setParameter("version", "3.4");
        }
    }

    public File getFile() throws IOException {
        File realUploadFolder = fileResourceMap.get("webRoot").getFile();
        File file = new File(realUploadFolder, filePath);
        return file;
    }

    @Test
    public void testGetFile() throws IOException {
        assertTrue(getFile().exists());
    }

    @Test
    public void testMock() throws IOException {
        initialize();

        MultipartFile file = mreq.getFile(fileName);
        assertNotNull(file);
    }

    @Test
    public void testUpload() throws IOException {
        initialize();

        UploadedFile uploadedFile = fileUploader.upload(mreq, fileName,
                new DefaultFileUploadPolicy("exampleImageTemp", fileName, new String[] { DateTime.now().toString("yyyyMMddHHmmssSSS") }));

        assertNotNull(uploadedFile);

        System.out.println("FileSize: " + uploadedFile.getFileSize());
        System.out.println("Path: " + uploadedFile.getPath());
        System.out.println("SourceFileName: " + uploadedFile.getSourceFileName());
        System.out.println("SourceFileNameOnly: " + uploadedFile.getSourceFileNameOnly());
        System.out.println("SourceFilePathOnly: " + uploadedFile.getSourceFilePathOnly());
        System.out.println("TargetFile: " + uploadedFile.getTargetFile());
        System.out.println("TargetFileNameOnly: " + uploadedFile.getTargetFileNameOnly());
        System.out.println("TargetFileRealPathOnly: " + uploadedFile.getTargetFileRealPathOnly());


        File file = new File(uploadResource.getFile() + "/temp/example/" + uploadedFile.getTargetFileNameOnly());
        assertTrue(file.exists());

        System.out.println(fileUploader.getWebPath(uploadedFile.getTargetFile().toPath()));
    }


    public byte[] createFileContent(byte[] data, String boundary, String contentType, String fileName) {
        String start = "--" + boundary + "\r\n Content-Disposition: form-data; name=\"file\"; filename=\"" + fileName + "\"\r\n"
                + "Content-type: " + contentType + "\r\n\r\n";
        ;

        String end = "--" + boundary + "--"; // correction suggested @butfly
        return ArrayUtils.addAll(start.getBytes(), ArrayUtils.addAll(data, end.getBytes()));
    }

    // static
    // {
    // Logger log = Logger.getRootLogger();
    // log.setLevel(Level.ALL);
    // log.addAppender(new ConsoleAppender(
    // new PatternLayout("%d{ISO8601} %-5p %m%n")));
    //// new PatternLayout("%-6r [%p] %c - %m%n")));
    // }
}
```
