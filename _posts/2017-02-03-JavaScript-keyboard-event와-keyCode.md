---
layout: post
date: 2017-02-03 13:38:10 +0900
title: 'JavaScript: keyboard event와 keyCode'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - json
  - literal
  - javascript object literal notation
---

자바스크립트로 키보드 이벤트 발생 시의 keycode를 확인하는 방법은:

```html
<script>
  // document.getElementById("tt")[0].onkeydown = function(event) {
  //   console.log('keydown - keyCode : ' + event.keyCode);
  //   console.log('keydown - which : ' + event.which);
  // };

  function keydownHandler(event) {
    var keyCode = event.keyCode ? event.keyCode : event.which;
    // which: 구 버전 파이어폭스는 호환 처리
    // 혹은
    keyCode = event.keyCode || event.which;
    console.log('keydown:' + keyCode);
  }

  function keypressHandler(event) {
    var keyCode = event.keyCode || event.which;
    console.log('keypress:' + keyCode);
  }
</script>

<input id="tt" type="text" onkeydown="keydownHandler(event)" onkeypress="keypressHandler(event)"/>
```

`event.which`는 구 버전 파이어폭스의 호환 처리를 위한 프로퍼티다. (과거에는 keypress 이벤트 시 keyCode가 항상 0이던 시절이 있었음)

## 입력 테스트

아래 창에서 아무 키나 입력해보자:

<table>
  <tr><td><input id="inptKeyCodeTest" type="text" style="border: 0"></td></tr>
</table>
<table id="tabKeyCodeTest">
  <tr>
    <td></td>
    <td>event.keyCode</td>
    <td>event.which</td>
  </tr>
  <tr>
    <td><p>onkeydown</p></td>
    <td id="result1"></td>
    <td id="result2"></td>
  </tr>
  <tr>
    <td><p>onkeypress</p></td>
    <td id="result3"></td>
    <td id="result4"></td>
  </tr>
  <tr>
    <td><p>onkeyup</p></td>
    <td id="result5"></td>
    <td id="result6"></td>
  </tr>
</table>
<script>
  document.getElementById('inptKeyCodeTest').onkeydown = function(e) {
    document.getElementById('result1').innerHTML = "<p>" + e.keyCode + "</p>";
    document.getElementById('result2').innerHTML = "<p>" + e.which + "</p>";
  };

  document.getElementById('inptKeyCodeTest').onkeypress = function(e) {
    document.getElementById('result3').innerHTML = "<p>" + e.keyCode + "</p>";
    document.getElementById('result4').innerHTML = "<p>" + e.which + "</p>";
  };

  document.getElementById('inptKeyCodeTest').onkeyup = function(e) {
    document.getElementById('result5').innerHTML = "<p>" + e.keyCode + "</p>";
    document.getElementById('result6').innerHTML = "<p>" + e.which + "</p>";
  };
</script>

## KeyCode Reference Table

[표 출처](https://lael.be/55)

* keydown 이벤트 발생 시의 keyCode이며 keypress의 경우 얻는 값이 다를 수 있으니 주의할 것.

| code | key          | code | key          | code | key       | code | key           | code | key          |
|------|--------------|------|--------------|------|-----------|------|---------------|------|--------------|
| 0    |              | 10   |              | 20   | Caps Lock | 30   |               | 40   | Arrow Down   |
| 1    |              | 11   |              | 21   |           | 31   |               | 41   |              |
| 2    |              | 12   |              | 22   |           | 32   |               | 42   |              |
| 3    |              | 13   | Enter        | 23   |           | 33   | Page Up       | 43   |              |
| 4    |              | 14   |              | 24   |           | 34   | Page Down     | 44   |              |
| 5    |              | 15   |              | 25   |           | 35   | End           | 45   | Insert       |
| 6    |              | 16   | Shift        | 26   |           | 36   | Home          | 46   | Delete       |
| 7    |              | 17   | Ctrl         | 27   | Esc       | 37   | Arrow Left    | 47   |              |
| 8    | Backspace    | 18   | Alt          | 28   |           | 38   | Arrow Up      | 48   | 0            |
| 9    | Tab          | 19   | Pause/Break  | 29   |           | 39   | Arrow Right   | 49   | 1            |
|      |              |      |              |      |           |      |               |      |              |
| 50   | 2            | 60   |              | 70   | f         | 80   | p             | 90   | z            |
| 51   | 3            | 61   | `=+`         | 71   | g         | 81   | q             | 91   | Windows      |
| 52   | 4            | 62   |              | 72   | h         | 82   | r             | 92   |              |
| 53   | 5            | 63   |              | 73   | i         | 83   | s             | 93   | Right Click  |
| 54   | 6            | 64   |              | 74   | j         | 84   | t             | 94   |              |
| 55   | 7            | 65   | a            | 75   | k         | 85   | u             | 95   |              |
| 56   | 8            | 66   | b            | 76   | l         | 86   | v             | 96   | 0 (Num Lock) |
| 57   | 9            | 67   | c            | 77   | m         | 87   | w             | 97   | 1 (Num Lock) |
| 58   |              | 68   | d            | 78   | n         | 88   | x             | 98   | 2 (Num Lock) |
| 59   | `;:`         | 69   | e            | 79   | o         | 89   | y             | 99   | 3 (Num Lock) |
| 100  | 4 (Num Lock) | 110  | `.` (Num Lock)| 120 | F9        | 130  |               | 140  |              |
| 101  | 5 (Num Lock) | 111  | `/` (Num Lock)| 121 | F10       | 131  |               | 141  |              |
| 102  | 6 (Num Lock) | 112  | F1           | 122  | F11       | 132  |               | 142  |              |
| 103  | 7 (Num Lock) | 113  | F2           | 123  | F12       | 133  |               | 143  |              |
| 104  | 8 (Num Lock) | 114  | F3           | 124  |           | 134  |               | 144  | Num Lock     |
| 105  | 9 (Num Lock) | 115  | F4           | 125  |           | 135  |               | 145  | Scroll Lock  |
| 106  | * (Num Lock) | 116  | F5           | 126  |           | 136  |               | 146  |              |
| 107  | + (Num Lock) | 117  | F6           | 127  |           | 137  |               | 147  |              |
| 108  |              | 118  | F7           | 128  |           | 138  |               | 148  |              |
| 109  | - (Num Lock) | 119  | F8           | 129  |           | 139  |               | 149  |              |
|      |              |      |              |      |           |      |               |      |              |
| 150  |              | 160  |              | 170  |           | 180  |               | 190  | `.>`         |
| 151  |              | 161  |              | 171  |           | 181  |               | 191  | `/?`         |
| 152  |              | 162  |              | 172  |           | 182  | My Computer   | 192  | ``` `~```         |
| 153  |              | 163  |              | 173  |           | 183  | My Calculator | 193  |              |
| 154  |              | 164  |              | 174  |           | 184  |               | 194  |              |
| 155  |              | 165  |              | 175  |           | 185  |               | 195  |              |
| 156  |              | 166  |              | 176  |           | 186  |               | 196  |              |
| 157  |              | 167  |              | 177  |           | 187  |               | 197  |              |
| 158  |              | 168  |              | 178  |           | 188  | `,<`          | 198  |              |
| 159  |              | 169  |              | 179  |           | 189  |               | 199  |              |
| 200  |              | 210  |              | 220  | `\|`      | 230  |               | 240  |              |
| 201  |              | 211  |              | 221  | `]}`      | 231  |               | 241  |              |
| 202  |              | 212  |              | 222  | `'"`      | 232  |               | 242  |              |
| 203  |              | 213  |              | 223  |           | 233  |               | 243  |              |
| 204  |              | 214  |              | 224  |           | 234  |               | 244  |              |
| 205  |              | 215  |              | 225  |           | 235  |               | 245  |              |
| 206  |              | 216  |              | 226  |           | 236  |               | 246  |              |
| 207  |              | 217  |              | 227  |           | 227  |               | 227  |              |
| 208  |              | 218  |              | 228  |           | 238  |               | 248  |              |
| 209  |              | 219  | `[{`         | 229  |           | 239  |               | 249  |              |
