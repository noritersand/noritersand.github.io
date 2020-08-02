---
layout: post
date: 2015-04-15 10:16:00 +0900
title: '[jQuery] jQuery.extend()로 플러그인 만들기'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - extend
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://api.jquery.com/jQuery.fn.extend/](https://api.jquery.com/jQuery.fn.extend/)

쫑알쫑알쫑알.

## jQuery.fn.extend()

함수를 확장하여 플러그인으로 사용한다.

```
jQuery.fn.extend( object )
```

- `object`: jQuery 프로토타입에 병합할 자바스크립트 오브젝트. (An object to merge onto the jQuery prototype.)

```html
<script src="http://code.jquery.com/jquery.min.js"></script>
<script>
jQuery.fn.extend({
  check: function() {
    return this.each(function() {
      this.checked = true;
    });
  },
  uncheck: function() {
    return this.each(function() {
      this.checked = false;
    });
  }
});

$(function() {
  $("#button1").click(function() {
    $('input').check();
  });

  $("#button2").click(function() {
    $('input').uncheck();
  });
});
</script>
<style>
div {
    background: yellow;
    margin: 5px;
}
span {
    color: red;
}
</style>

</head>
<body>
    <form>
        <input type="checkbox" name=""> <input type="checkbox" name="">
        <input type="checkbox" name=""> <input type="checkbox" name="">
        <input type="checkbox" name=""> <input type="checkbox" name="">
    </form>
    <input type='button' id='button1' value='전체선택'>
    <input type='button' id='button2' value='전체해제'>
</body>
```

`jQuery.fn.extend()`는 사실 jQuery.fn과 `jQuery.extend()`의 합성한 메서드다.

## jQuery.fn

```
jQuery.fn.functionName = Function
```

- `functionName`: 생성할 함수의 이름
- `Functionl`: 자바스크립트 함수 정의. (function Literal)

fn은 jQuery 셀렉터를 이용하기 위해 사용된다. fn이 없을 경우 일반적인 자바스크립트 함수와 동일하다.

```js
$.testFn = function(msg) {
  alert(msg);
};

$.testFn("waldo");
```

### example 1

```html
<input type="text" name="memberName" id="memberName" placeholder="이름입력">
<script>
$(function() {
  $.fn.fnTest = function() {
    // this: jQuery selector가 지정한 객체
    alert(this.get(0).tagName);
  };

  $("#memberName").fnTest();
});
</script>
```

### example 2

```html
<div id="result"></div>
<script>
$(function() {
  $.fn.hello = function(args) {
    var msg = args.message1;
    msg += "<br>" + args.message2;

    // this: jQuery selector로 선택된 객체
    args.callback(msg, this);

    console.log(args.lengthTest.length); // 0
    console.log(args.unnamed.length);  // TypeError: args.unnamed is undefined
  };

  $("#result").hello({
    message1: "Hello there! Mighty fine morning.",
    message2: "if you ask me. I'm Waldo.",
    lengthTest: "",
    callback: function(msg, target) {
      $(target).html(msg).css("font-weight", "bold");
    }
  });
});
</script>
```

### example: 노드에 이벤트 핸들러 등록

```html
<input type="text" placeholder="이름입력">
<button type="button">push me</button>
<script>
$(function() {
    $.fn.hello = function() {
        // this: jQuery 객체 배열
        this.each(function(index, element) {
          $(element).click(function() {
              var msg = "if you ask me. i'm ";
              msg += $(this).prev().val(); // this = JavaScript Object
              alert(msg);
          });
        });
    };
    $("button").hello();
});
</script>
```

### example: 이벤트 바인딩 방식과 함수 직접 호출을 같이 적용

```html
<button type="button">fire!</button>
<script>
$.fn.alert = function(option) {
  // 이벤트 핸들러 바인딩
  this.each(function(index, element) {
    $(element).click(function() {
        $.fireAlert(option);
      });
  });
};
// 함수 구현
$.fireAlert = function(option) {
  alert(option.message);
}

$.fireAlert({ message: '문서 로딩 직후 나타나는 경고창' }); // 함수 직접 호출
$('button').alert({ message: '버튼 클릭 시 나타나는 경고창' }); // 이벤트 바인딩
</script>
```

## jQuery.extend()

`extend()`는 말 그대로 객체를 확장하는 메서드다. 첫 번째 인자로 지정된 객체에 두 번째, 혹은 그 이상의 인자로 지정된 객체를 덮어 쓴다. 동일한 이름의 프로퍼티가 존재할 땐 가장 나중에 오는 객체가 우선권이 높다.

```
jQuery.extend( target [, object1 ] [, objectN ] )
jQuery.extend( [deep ], target, object1 [, objectN ] )
```

- `target`: ?
- `object1`: ?
- `objectN`: ?
- `deep`: If true, the merge becomes recursive (aka. deep copy). On a deep extend, Object and Array are extended, but object wrappers on primitive types such as String, Boolean, and Number are not. Deep-extending a cyclical data structure will result in an error.

```js
var object = $.extend({}, object1, object2);
```

첫 번째 오브젝트에 나중에 오는 오브젝트들을 병합한다. 위의 경우 아무것도 없는 오브젝트 `{}`에 object1과 object2를 차례대로 병합하는 것이 되겠다. 리턴되는 오브젝트는 병합 결과를 저장한 첫 번째 오브젝트를 의미한다. 다만 첫 번째 오브젝트를 이름이 없는 `{}`로 작성했으므로 병합 결과에 접근할 수 있도록 반드시 변수에 실행결과를 저장해야 한다.

```js
var object = $.extend(object1, object2);
object === object1; // true
```

이 경우 병합 결과가 첫 번째 오브젝트인 object1에 저장되므로 object와 object1은 서로 일치한다. 따라서 변수로 사용된 object는 생략할 수 있다.

deep 옵션이 지정되지 않았거나 false 일 때는 'non recursive merge'로 동작하는데 이 방식은 오브젝트의 프로퍼티가 또 다른 오브젝트일 때 복사하지 않는다.

### example

```html
<div id="log"></div>

<script>
var object1 = {
  apple: 0,
  banana: { weight: 52, price: 100 },
  cherry: 97
};
var object2 = {
  banana: { price: 200 },
  durian: 100
};

// Merge object2 into object1, recursively
$.extend( true, object1, object2 );

// Assuming JSON.stringify - not available in IE<8
$( "#log" ).append( JSON.stringify( object1 ) );
</script>
```
