---
layout: post
date: 2014-07-23 17:39:00 +0900
title: '[Java] URLConnection'
categories:
  - java
tags:
  - java
  - urlconnection
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

서버사이드 통신 모듈. **수정 필요함.**

명시된 URL을 호출하여 해당 페이지의 응답값을 문자열로 반환한다. 응답값이 통으로 된 HTML인 경우 어디선가 에러가 나지만 귀찮아서 그냥 둔다.... 따라서 파라미터 형식의 값을 주고 받을 때만 사용할 것.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.HashMap;

import javax.net.ssl.HostnameVerifier;
import javax.net.ssl.HttpsURLConnection;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSession;
import javax.net.ssl.TrustManager;
import javax.net.ssl.X509TrustManager;

import org.json.simple.JSONObject;

/**
 * URL CONNECTION CLASS
 */
public class URLConnection {
    private final static String CONNECT_TIMEOUT = "5000";
    private final static String READ_TIMEOUT = "5000";
    public final static String CONTENT_TYPE_JSON = "application/json";
    public final static String CONTENT_TYPE_DEFAULT
            = "application/x-www-form-urlencoded";

    /**
     * <pre>
     * 기본 get 방식 호출 메서드
     *  ㄴ method = "get",
     *  ㄴ contentType = "application/x-www-form-urlencoded" </pre>
     * @param url
     * @param encoding
     * @return
     * @throws IOException
     * @throws Exception
     */
    public String getStringFromURL(String url, String encoding)
            throws IOException, Exception {
        return getStringFromURL(url, "get", encoding, CONTENT_TYPE_DEFAULT, "");
    }

    /**
     * 파라미터가 쿼리스트링일 때 사용
     * @param url
     * @param method GET|POST
     * @param encoding
     * @param contentType DEFAULT|JSON
     * @param queryString 파라미터 문자열
     * @return
     * @throws Exception
     */
    public String getStringFromURL(String url, String method, String encoding,
            String contentType, String queryString) throws Exception {
        return getStringFromURL(url, method, encoding, contentType, (Object) queryString);
    }

    /**
     * 파라미터가 맵 타입일 때 사용
     * @param url
     * @param method GET|POST
     * @param encoding
     * @param contentType DEFAULT|JSON
     * @param params 파라미터 맵
     * @return
     * @throws Exception
     */
    public String getStringFromURL(String url, String method, String encoding,
            String contentType, HashMap<String, String> params) throws Exception {
        return getStringFromURL(url, method, encoding, contentType, (Object) params);
    }

    private String getStringFromURL(String url, String method, String encoding,
            String contentType, Object params) throws Exception {
        String parameterStr = "";

        if (params instanceof HashMap) {
            @SuppressWarnings("unchecked")
            HashMap<String, String> map = (HashMap<String, String>) params;

            StringBuffer parameter = new StringBuffer("");

            for (String key : map.keySet()) {
                // log.debug("key="+key);
                parameter.append(key).append("=").append((String) map.get(key)).append("&");
            }
            parameterStr = parameter.deleteCharAt(parameter.length() -1).toString();

        } else if (params instanceof String) {
            parameterStr = (String) params;
        }

        boolean isPost = false;
        if ("post".equals(method)) {
            isPost = true;
        }

        if (!isPost) {
            url = "".equals(parameterStr) ? url
                    : url + ((url.indexOf("?") != -1) ? "&" : "?") + parameterStr;
        }

        URL apiURL = new URL(url);
        HttpsURLConnection conn_https = null;
        HttpURLConnection conn = null;
        BufferedReader br = null;
        StringBuffer result = new StringBuffer();
        BufferedWriter bw = null;

        try {
            if (url.substring(0, 5).toLowerCase().equals("https")) {

                TrustManager[] trustAllCerts = new TrustManager[] { new X509TrustManager() {

                    public java.security.cert.X509Certificate[] getAcceptedIssuers() {
                        return null;
                    }

                    public void checkClientTrusted(java.security.cert
                            .X509Certificate[] certs, String authType) {
                        // No need to implement.
                    }

                    public void checkServerTrusted(java.security.cert
                            .X509Certificate[] certs, String authType) {
                        // No need to implement.
                    }
                } };

                SSLContext sc = SSLContext.getInstance("SSL");
                sc.init(null, trustAllCerts, new java.security.SecureRandom());

                HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
                // conn_https.setDefaultSSLSocketFactory(sc.getSocketFactory());
                HostnameVerifier allHostsValid = new HostnameVerifier() {
                    public boolean verify(String hostname, SSLSession session) {
                        return true;
                    }
                };

                HttpsURLConnection.setDefaultHostnameVerifier(allHostsValid);
                conn_https = (HttpsURLConnection) apiURL.openConnection();
                conn_https.setDoOutput(true);

                if (isPost) {
                    conn_https.setRequestMethod("POST");
                    conn_https.setRequestProperty("Content-Type", contentType);
                } else {
                    conn_https.setRequestMethod("GET");
                }

                conn = conn_https;

            } else {
                conn = (HttpURLConnection) apiURL.openConnection();
                conn.setConnectTimeout(Integer.parseInt(CONNECT_TIMEOUT));
                conn.setReadTimeout(Integer.parseInt(READ_TIMEOUT));
                conn.setDoOutput(true);

                if (isPost) {
                    conn.setRequestMethod("POST");
                    conn.setRequestProperty("Content-Type", contentType);
                } else {
                    conn.setRequestMethod("GET");
                }
                conn.connect();
            }

            if (isPost) {
                // parameterStr = URLEncoder.encode(parameterStr, "UTF-8");
                bw = new BufferedWriter(new OutputStreamWriter(conn.getOutputStream(),
                        "UTF-8"));
                bw.write(parameterStr);
                bw.flush();
                bw = null;
            }

            br = new BufferedReader(new InputStreamReader(conn.getInputStream(),
                    encoding));
            String line = null;

            while ((line = br.readLine()) != null) {
                result.append(line + "\n");
            }

        } catch (Exception e) {
            throw e;
        } finally {
            if (conn != null) {
                conn.disconnect();
            }
            if (conn_https != null) {
                conn_https.disconnect();
            }
            if (br != null) {
                br.close();
            }
            if (bw != null) {
                bw.close();
            }
        }

        return result.toString();
    }

    public static void main(String[] args) throws Exception {
        URLConnection urlConn = new URLConnection();

        // testing #1
        String result = urlConn.getStringFromURL("http://test.url",
                "get", "utf-8", URLConnection.CONTENT_TYPE_DEFAULT, "test=value");
        System.out.println(result);

        // testing #2
        JSONObject jsonOb = new JSONObject();
        jsonOb.put("property01", "value1");
        String result2 = urlConn.getStringFromURL("http://test.url",
                "post", "utf-8", URLConnection.CONTENT_TYPE_JSON, jsonOb.toString());
        System.out.println(result2);
    }
}
```

끗
