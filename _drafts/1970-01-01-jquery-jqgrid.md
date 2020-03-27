---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[jQuery] jqGrid'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - jqgrid
---
![](/images/jqgrid-1.png)

#### 관련 문서

- [http://www.trirand.com](http://www.trirand.com)
- [github](https://github.com/tonytomov/jqGrid/tree/master)
- [wiki](http://www.trirand.com/jqgridwiki/doku.php?id=wiki:jqgriddocs)
- [demo](http://trirand.com/blog/jqgrid/jqgrid.html)

jqGrid는 데이터 리스트를 표현하는 grid 플러그인이다. 페이징, 셀의 수정/추가/삭제 등 다양한 기능을 지원한다.

## 수정모드 강제종료

jqGrid에서 특정셀을 수정 가능하도록 설정(colmodel option - editable: true)한 경우 해당셀을 선택하면 옵션에 따라 `<input>` 혹은 `<select>` 등의 입력폼으로 바뀌게 되어 있다.

![](/images/jqgrid-2.png)

위 사진처럼 입력폼으로 바뀐 셀의 상태를 수정모드라고 했을 때, 수정모드를 종료하지 않고 해당 셀의 값을 가져오려고 하면 화면상에 보이는것 처럼 200이 아닌 다른값이 나온다. (이 값은 empty string이나 입력폼 태그 자체가 나올 수도 있다)

따라서 만약 셀을 수정하고 그 데이터를 submit등으로 처리해야 한다면 반드시 셀들의 현 상태를 체크하는 코드가 선행되어야 한다. (물론 사용자가 엔터 등으로 수정모드를 손수 종료시키고 다음 행동을 한다면 문제가 없겠지만...) 셀을 수정중인 상태에서 아래 코드를 실행시켜보면 수정모드가 즉시 종료되는것을 확인할 수 있다.

```js
// selRows = $('#list').getGridParam().selarrrow;  // 선택한row만 적용할 경우
var selRows = $('#list').getDataIDs();  // 모든row
var colModel = $('#list').getGridParam().colModel;
var gridData = [];

for(var i=0; i<selRows.length; i++) {
  for(var j=1; j<colModel.length; j++) {
    if($('#list'+' tr#'+selRows[i]+' td:eq('+j+')').hasClass('edit-cell')){

      $('#list').saveCell(selRows[i], j);
      // 혹은
      // $('#list').jqGrid('saveCell',selRows[i],j);
    }
  }
  var rowData = $('#list').getRowData(selRows[i]);
  gridData.push(rowData);
}
```

## onSelectRow, onCellSelect, onSelectCell의 차이

[http://www.trirand.com/jqgridwiki/doku.php?id=wiki:events](http://www.trirand.com/jqgridwiki/doku.php?id=wiki:events)

![](/images/jqgrid-3.png)

셋 다 이벤트 발생 시점은 grid에 mouseclick, 혹은 keydown 이벤트가 발생했을때로 동일하지만 적용범위에서 차이가 있다:
- onSelectRow는 체크박스(multiselect: true)에 마우스 클릭했을 때 작동한다. 전체선택에 해당되는 머리글의 체크박스는 제외.
- onCellSelect는 머리글 셀이 아닌 모든 셀을 클릭했을 때 작동한다. 체크박스가 있는 셀에서 체크박스가 아닌 나머지 셀의 부분을 클릭해도 onCellSelect이벤트가 발생한다.
- onSelectCell는 머리글 셀이 아니고, 체크박스가 아니며, 수정가능한 셀이 아닌 데이터 셀을 클릭했을 때만 작동한다. 클릭 시 에디트 모드로 바뀌는 셀 colModel: {editable: true}
- 만약 데이터 셀이지만 수정가능한 셀이라면 onSelectCell은 작동하지 않고 onCellSelect만 작동한다.
- 키보드로 셀 이동 중일 때 작동하는 것은 onSelectCell
- 셀이 선택되었을 때 수정가능한 셀이라도 엔터키 입력만으로는 아무것도 작동하지 않는다.
- 머리글 셀의 모두선택 체크박스를 클릭했을 때는 onSelectAll이 작동한다.
- onCellSelect 콜백 파라미터인 e는 자바스크립트 event객체다.
