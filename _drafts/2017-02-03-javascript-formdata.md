---
layout: post
date: 2017-02-03 13:42:00 +0900
title: '[JavaScript] FormData'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - formdata
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/API/FormData](https://developer.mozilla.org/ko/docs/Web/API/FormData)

## FormData

```
var formData = new FormData([ formName ]);
```

- `formName`: 문서에 존재하는 form 요소. 명시할 경우 블라블라

쫑알쫑알

## example

asd

## payload로 던질 때의 값

`FormData`를 xhr 통신에 사용하면 payload 값이 이상하게(chunk로 추정되는?) 넘어간다.

가령 소스가:

```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    console.log('xhr:', xhr);
  }
};

// post method#2
let data = new FormData();
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
