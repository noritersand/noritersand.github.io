---
title: 'JavaScript: JSONP(JSON with Padding)'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
---

원작성자: 한장희

JSONP는 XMLHttpRequest()가 동일 서버에만 접근할 수 있다는 한계점(동일 출처 정책, same origin policy)을 극복하여 동적으로 데이터를 서버에 전달하고 응답값을 받아서 처리하기 위해서 고안된 기술이다. 특별하거나 새로운 기술이라기 보다는 편법으로 만든 기술이기 때문에 XMLHttpRequest()와 동일한 기능을 수행할 수는 없다. 기본적으로 get 방식의 호출만 가능하기 때문에 많은 한계를 가질 수 밖에 없다.

HTML에서 자바스크립트 파일을 호출하기 위해선 <script src="xx.js"></script> 나 자바스크립트 다음과 같은 형식으로 호출할 수 있다.

var script = document.createElement('script');
script.src = 'http://localhost/jsonp.js';
document.getElementsByTagName('head')[0].appendChild(script);
DOM에 script라는 객체를 생성한 후 거기에 외부 소스를 불러서 집어 넣고, 그걸 <head> tag에 집어 넣은 것이다. 스크립트 소스는 html 페이지가 있는 것과 다른 서버의 것을 불러올 수 있다는 걸 이용해서 마치 외부 스크립트를 불러오는 것처럼 하는 것이다. 예를 들어 스크립트 소스 주소를 "http://localhost/test.jsp?id=1&name=upia" 식으로 해서 스크립트를 불러오는 것이다. 그럼 서버에서는 그걸 받아서 자바스크립트를 전송해 준다. 그 안에 JSON 형식의 데이터도 넣어서 말이다. 클라이언트는 그걸 받아서 그 안에 있는 자바스크립트를 실행한다. 그리고 그 자바스크립트는 어떤 함수를 호출하고, 그 함수에서 실질적인 데이터 처리를 한다.

단점은 JSONP를 위한 뭔가 기능이 존재하는 게 아니라 편법이다 보니 지원되는 기능이 없다는 것이다. 자바스크립트 호출을 중단시킬 수도, 호출 결과를 알 수도 없고 GET 방식이어서 보내는 데이터의 크기도 제한된다. 따라서 XMLHttpRequest()의 모든 기능을 사용하려면 프록시가 존재하는 수 밖에 없을 것이다.

## TODO: jQuery 말고 JavaScript로 코드 수정할 것

### request
정상적인 데이터를 돌려받기 위해서는 아래와 같이 작성해야 한다.
```js
$.getJSON("http://127.0.0.1:8080/jsp/upload?id=user&callback=?"
        , function(data) {
    alert(data.result + ", " +  data.go);
});
```
혹은
```js
$.ajax({
    url: "http://127.0.0.1:8080/jsp/upload",
    data: "id=user",
    dataType: "jsonp",
    jsonp: "callback",
    success: function(data) {
        if(data != null) {
            alert(data.result + ', ' +  data.go);
        }
    }
});
```

### response
```java
protected void doGet(HttpServletRequest req, HttpServletResponse res)
        throws ServletException, IOException {
    req.setCharacterEncoding("UTF-8");
    res.setCharacterEncoding("UTF-8");

    String id = req.getParameter("id");
    String callBack = req.getParameter("callback");

    JSONObject obj = new JSONObject();
    obj.put("result", id);
    obj.put("go", "테스트");

    String resStr = callBack + "(" + obj.toString() + ")";
    // jQuery1111042987291840321584_1406288916210({"result":"user","go":"테스트"})

    PrintWriter out = res.getWriter();
    out.write(resStr);
    out.flush();
    out.close();
}
```
