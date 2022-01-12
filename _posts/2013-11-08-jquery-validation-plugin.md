---
layout: post
date: 2013-11-08 11:44:26 +0900
title: '[jQuery] jQuery Validation plugin'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - validation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://jqueryvalidation.org/documentation/](http://jqueryvalidation.org/documentation/)
- [http://jqueryvalidation.org/documentation/#link-demos](http://jqueryvalidation.org/documentation/#link-demos)
- [https://github.com/jzaefferer/jquery-validation/releases](https://github.com/jzaefferer/jquery-validation/releases)


#### 필수 라이브러리

- jquery 1.6.4 or higher

## validate()

```
validate( { options } )
```

- `debug`: true일 경우 validation 후 submission을 수행하지 않음. (default: false)
- `onfocusout`: onblur 시 해당항목을 validation 할 것인지 여부 (default: true)
- `rules`: 각 항목[^1] 별로 validation rule을 지정한다.
- `messages`: rules에서 정의된 조건으로 validation에 실패했을 때 화면에 표시할 메시지를 지정한다.
- `errorPlacement`: validator는 기본적으로 validation 실패 시 실패한 노드 우측에 실패 메시지를 표시하게 되어있다. 작동을 원하지 않으면 내용이 없는 errorPlacement를 선언한다.
- `invalidHandler`: validation 실패 시의 핸들러를 정의한다. 위의 경우 실패 메시지를 alert으로 표시하도록 되어 있다.
- `submitHandler`: 유효성 검사가 완료된 뒤 수행할 핸들러를 정의한다. 이 옵션을 명시할 경우 'submit' 이벤트만 발생하기 때문에 formdata의 전송은 별도로 명시해야 한다.

`validate()`는 유효성 검사 대상과 항목, 작동 등을 정의한다.

다음은 searchForm에서 submit 이벤트가 발생할 때 플러그인이 자동으로 작동하게 하는 예시다:

```js
$(function() {
  $('#searchForm').validate({
    debug: false,
    onfocusout: false,
    rules: {
      searchValue: {
        required: true,
        maxlength: 10
      }
    }, messages: {
      searchValue: {
        required: '검색조건은 필수값입니다.',
        maxlength: $.validator.format('{0}자 내로 입력하세요.')
      }
    }, errorPlacement: function(error, element) {
      // do nothing
    }, invalidHandler: function(form, validator) {
       var errors = validator.numberOfInvalids();
       if (errors) {
         alert(validator.errorList[0].message);
         validator.errorList[0].element.focus();
       }
    }
  });
});
```

## valid()

설정한 validation의 트리거.

```js
$('#searchForm').valid();
```

앞서 `validate()` 부분에서 잠시 언급했지만 기본적으로 `validate()` 메서드는 jQuery 셀렉터로 선택된 HTML 노드에 이벤트 핸들러를 할당하며 이후 'submit' 이벤트가 발생하면 JSON으로 작성된 옵션과 함께 플러그인을 실행한다. 따라서 우리는 `valid()`를 호출하지 않고도 `submit()` 만으로도 플러그인을 작동시킬 수 있다.

`valid()` 메서드가 호출되고 validation 결과가 true일 때 validator는 submitHandler를 실행한다. 다음을 보자:

```js
function search() {
  $('#searchForm').submit();
}

// document on ready callback function
$(function() {
  $('#searchForm').validate({
    onfocusout: false,
    rules: {
      searchValue: {
        required: true,
        maxlength: 10
      }
    }, messages: {
      searchValue: {
        required: '검색조건은 필수값입니다.',
        maxlength: $.validator.format('{0}자 내로 입력하세요.')
      }
    }, errorPlacement: function(error, element) {
      // do nothing
    }, invalidHandler: function(form, validator) {
       var errors = validator.numberOfInvalids();
       if (errors) {
         alert(validator.errorList[0].message);
         validator.errorList[0].element.focus();
       }
    }, submitHandler: function(form) {
      alert('handler invoke');
    }
  });
});
```

## rules

### required

입력 필수 항목설정. text, password, select, radio, checkbox type에 사용된다.  ex) `required: true`

다른 필드에 의존되어 적용하는 경우:

```html
<script>
  $('#searchForm').validate({
    rules: {
      searchValue: {
        required: "#other:checked"
      }
    }
    // 중략
  });
</script>

<form id="searchForm">
    <input type="text" name="searchValue"/>
    <input type="checkbox" name="other" id="other"/>
</form>
```

checkbox가 체크 되기 전에는 required는 작동하지 않는다. `#other`의 `#`을 보면 알겠지만 jQuery selector expression으로 의존하는 필드를 찾게 되어 있다. 따라서 아이디가 아니라 name 속성이 other인 필드에 의존하려면 `required: "[name=other]"`라고 작성하면 된다.

```html
<script>
  $('#ageForm').validate({
    debug: true,
     rules: {
       age: {
         required: true
       },
       parent: {
         required: function(element) {
           return $("#age").val() < 13;
        }
      }
    }
  });
</script>

<form id="ageForm">
    age: <input type="text" name="age" id="age"/><br>
    parent: <input type="text" name="parent"/>
    <input type="submit" value="valid"/>
</form>
```

이 경우 age의 값이 1-12 일 때만 parent를 체크

### remote

외부 URL을 이용한 validation이 필요한 경우 사용한다.

```js
$("#myform").validate({
  rules : {
    emailAddress : {
      required : true,
      email : true,
      remote : {
        url : "check-email.php",
        type : "post",
        data : {
          username : function() {
            return $("#username").val();
          }
        }
      }
    }
  }
});
```

### equalTo

다른 FORM 항목과 동일한 값인지 체크한다.

```html
<script>
  // document on ready callback function
  $(function() {
    $("#myForm").validate({
      rules: {
        password: "required",
        passwordConf: {
          equalTo: "#password"
        }
      }
    });
  });
</script>

<form id="myForm">
    비밀번호: <input type="text" name="password" id="password"/><br>
    비밀번호확인: <input type="text" name="passwordConf"/>
    <input type="submit" value="valid"/>
</form>
```

### minlength

최소 길이 체크.

```js
minlength: 3
```

### maxlength

최대 길이 체크.

```js
maxlength: 10
```

### rangelength

길이 범위 체크.

```js
rangelength[2, 6] // 2글자 이상 6글자 이하
```

### min

숫자의 최솟값 체크.

```js
min: 13 // 13보다 작을 경우 false
```

### max

숫자의 최댓값 체크.

```js
max: 5 // 5보다 클 경우 false
```

### range

숫자의 범위 체크.

```js
range: [13, 24] // 13보다 작거나 24보다 클 경우 false
```

### email

이메일 형식의 값인지 체크.

```js
email: true
```

### url

유효한 url 형식인지 체크.

```js
url: true
```

### date

유효한 날짜 형식의 값인지 체크

### dateISO

유효한 국제표준 날짜 형식인지 체크.

```js
dateISO: true
```

### number

유효한 숫자인지 체크.

```js
number: true
```

### digits

유효한 digit 값인지 체크. number와 다른점은 양의 정수만 허용한다. 즉, 소수와 음수일 경우 false.

### creditcard

유효한 카드번호 형식인지 체크. 공식페이지에서는 creditcard rule을 그대로 적용하지 말고 현지 사정에 맞게 수정하라고 권장한다.

```js
creditcard: true
```

### custom rule

이 외에 찾는 rule이 없다면 다음처럼 custom rule을 작성한다:

```js
$.validator.addMethod("domain", function(value, element) {
  return this.optional(element) || /^http:\/\/mycorporatedomain.com/.test(value);
}, "Please specify the correct domain for your documents");

$.validator.addMethod("math", function(value, element, params) {
  return this.optional(element) || value == params[0] + params[1];
}, $.validator.format("Please enter the correct value for {0} + {1}"));
```

## example

```html
<h1>jQuery form validation plugin sample</h1>
<form id="validationForm" name="validationForm">
    이름 <input type="text" name="name"><br>
    비밀번호 <input type="text" name="pass" id="pass"><br>
    비밀번호확인 <input type="text" name="confirm_pass"><br>
    이메일 <input type="text" name="email"><br>
    출생년도 <input type="text" name="birthdate"><br>
    전화번호 입력 <input type="checkbox" name="telagree" id="telagree" onclick="$(this).next().toggle();">
    <input type="text" name="tel" style="display: none;"><br>
    홈페이지 <input type="text" name="homepage"><br>
    <button type="button" onclick="$(this.form).submit()">SUBMIT</button>
</form>
</body>

<script>
  // custom validation 정의
  $.validator.addMethod(
    'mobilephone', function (value, element) {
      return (value.substring(0, 1) == 0) ? true : false;
    }, '휴대전화 번호는 0 으로 시작하여야 합니다.'
  );

  $('#validationForm').validate({
    onfocusout: false,
    rules: {
      name: {
        required: true,
        minlength: 2
      }, pass: {
        required: true,
        rangelength: [5, 12]
      }, confirm_pass: {
        required: true,
        rangelength: [5, 12],
        equalTo: '#pass'
      }, email: {
        required: true,
        email: true
      }, birthdate: {
        required: true,
        number: true,
        range: [1900, 2013]
      }, tel: {
        required: function (element) {
          return $('#telagree').is(':checked');
        },
        minlength: 10,
        mobilephone: true
      }, homepage: {
        url: true
      }
    }, messages: {
      name: {
        required: "이름을 입력하세요.",
        minlength: $.validator.format("이름은 최소 {0} 글자 이상 입력하세요.")
      }, pass: {
        required: "패스워드를 입력하세요.",
        rangelength: $.validator.format("패스워드 최소 {0}글자 이상 {1}글자 이하로 입력하세요.")
      }, confirm_pass: {
        required: "패스워드 확인 입력하세요.",
        rangelength: $.validator.format("패스워드 확인은 최소 {0}글자 이상 {1}글자 이하로 입력하세요."),
        equalTo: "패스워드 항목과 일치하지 않습니다."
      }, email: {
        required: "이메일을 입력하세요",
        email: "올바른 이메일 주소가 아닙니다."
      }, birthdate: {
        required: "출생 년도를 입력하세요.",
        number: "출생년도는 숫자로 입력하셔야합니다.",
        range: $.validator.format("출생년도는 {0}년에서 {1}년 사이의 값을 입력하세요.")
      }, tel: {
        required: "전화번호를 입력하세요.",
        minlength: $.validator.format("전화번호는 최소 {0} 글자 이상 입력하세요.")
      }, homepage: {
        url: "올바른 홈페이지 URL이 아닙니다."
      }
    }, errorPlacement: function (error, element) {
      // $(element).removeClass('error');
      // do nothing;
    }, invalidHandler: function (form, validator) {
      var errors = validator.numberOfInvalids();
      if (errors) {
        alert(validator.errorList[0].message);
        validator.errorList[0].element.focus();
      }
    }, submitHandler: function (form) {
      $.ajax({
        type: "POST",
        url: "/sample/ajax/ajaxJson.do",
        data: $(form).serialize(),
        dataType: "json",
        contentType: "application/x-www-form-urlencoded; charset=utf-8",
        success: function (data) {
          if (data.code == '0') {
            alert('code:' + data.code + '\n' + 'msg:' + data.msg);
          }
        }, error: function (jqXHR, textStatus, errorThrown) {
          alert(failMsg + ' ' + textStatus.msg);
        }
      });
    }
  });
</script>
```

[^1]: input, textarea 같은 필드를 의미하며 name 속성이 기준이다.
