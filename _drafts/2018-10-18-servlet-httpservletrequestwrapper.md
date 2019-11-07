---
layout: post
date: 2018-10-18 15:43:00 +0900
title: '[Servlet] HttpServletRequestWrapper'
categories:
  - servlet
tags:
  - servlet
  - java
  - httpservletrequest
---

* Kramdown table of contents
{:toc .toc}

HttpRequest 구현부를 교체할 때 사용한다.

대충 요딴식으로:

```java
import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HTMLTagFilter implements Filter {
	@SuppressWarnings("unused")
	private FilterConfig config;

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// HttpRequest 구현부(implementaion) 변경
		chain.doFilter(new HTMLTagFilterRequestWrapper((HttpServletRequest) request), response);
	}

	@Override
	public void init(FilterConfig config) throws ServletException {
		this.config = config;
	}

	@Override
	public void destroy() {

	}
}
```

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

/**
 * {@link javax.servlet.http.HttpServletRequestWrapper}를 확장한 HttpRequest 구현부
 */
public class HTMLTagFilterRequestWrapper extends HttpServletRequestWrapper {

	public HTMLTagFilterRequestWrapper(HttpServletRequest request) {
		super(request);
	}

	@Override
	public String[] getParameterValues(String parameter) {
		String[] values = super.getParameterValues(parameter);
		// 재정의할 메서드 본문
		return values;
	}

	@Override
	public String getParameter(String parameter) {
		String value = super.getParameter(parameter);
		// 재정의할 메서드 본문
		return value;
	}
}
```

필터를 이용하면 됨.

끗.
