---
layout: post
date: 2017-02-03 13:42:00 +0900
title: '[JavaScript] FormData'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - formdata
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)


## FormData

```
var formData = new FormData([ formName ]);
```

- `formName`: 문서에 존재하는 form 요소. 명시할 경우 블라블라

쫑알쫑알


## 인스턴스 메서드

- FormData.prototype.append()
- FormData.prototype.delete()
- FormData.prototype.entries()
- FormData.prototype.get()
- FormData.prototype.getAll()
- FormData.prototype.has()
- FormData.prototype.keys()
- FormData.prototype.set()
- FormData.prototype.values()


## example

asd


## payload로 던질 때의 값

`FormData`를 xhr 통신에 사용하면 payload 값이 chunk로 추정되는 형태로 전송된다.

가령 소스가:

```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    console.log('xhr:', xhr);
  }
};

// post method#2
var data = new FormData();
data.append('b', 2);
data.append('c', 3);
xhr.open('post', '/test/uncategorized/read-body.data?a=1');
xhr.setRequestHeader('Content-type', 'application/json');
xhr.setRequestHeader('Accept', 'application/json, text/javascript, */*; q=0.01');
xhr.send(data); // 바디로 읽어야 함
```

위와 같을 때 실제 넘어가는 값은:

```
// 파폭
-----------------------------30333176734664
Content-Disposition: form-data; name="b"

2
-----------------------------30333176734664
Content-Disposition: form-data; name="c"

3
-----------------------------30333176734664--
```

```
// 크롬
------WebKitFormBoundaryFMd1KVEfUx3bpgrQ
Content-Disposition: form-data; name="b"

2
------WebKitFormBoundaryFMd1KVEfUx3bpgrQ
Content-Disposition: form-data; name="c"

3
------WebKitFormBoundaryFMd1KVEfUx3bpgrQ--
```

요딴식이다.
