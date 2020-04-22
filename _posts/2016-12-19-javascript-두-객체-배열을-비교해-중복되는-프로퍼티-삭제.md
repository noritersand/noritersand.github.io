---
layout: post
date: 2016-12-19 11:29:00 +0900
title: '[JavaScript] 두 객체 배열을 비교해 중복되는 프로퍼티 삭제'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - property
  - object
  - compare
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```js
/**
 * target에서 origin과 일치하는 프로퍼티를 찾아 제거하고 남은 배열을 돌려준다.
 * targetKeys와 originKeys를 기준으로 일치하는지를 판단한다.
 * target과 origin이 '[{a = 1, b = 2}]' 같은 형태의 object array일 때만 정상 작동한다.
 *
 * example: removeDupProp(target, origin, 'loginId|userName', 'loginId|userName');
 *
 * @param target (object) origin과 비교해 중복을 제거할 대상
 * @param origin (object) 비교 대상
 * @param targetKeys (string) 비교 기준이되는 프로퍼티의 이름을 지정한다. 하나 이상일땐 파이프로 구분한다.
 * @param originKeys (string) target과 origin의 프로퍼티가 다를 때 지정한다. 생략하면 targetKeys를 같이 사용한다.
 * @returns array target에서 중복된 프로퍼티를 제거한 결과
 */
function removeDupProp(target, origin, targetKeys, originKeys) {
  var targetKeyArr = targetKeys.split('|');
  var originKeyArr = originKeys && originKeys.split('|') || targetKeyArr;
  if (targetKeyArr.length != originKeyArr.length) {
    throw new Error('targetKey와 originKey의 길이가 다릅니다.');
  }
  var keysLen = targetKeyArr.length;
  for (var i = 0; i < target.length; i++) {
    for (var j = 0; j < origin.length; j++) {
      var equalCount = 0;
      for (var k = 0; k < keysLen; k++) {
        if (target[i][targetKeyArr[k]] == origin[j][originKeyArr[k]]) {
          equalCount++;
        }
      }
      if (equalCount == keysLen) {
        target.splice(i, 1);
        i--;
        break;
      }
    }
  }
  return target;
}
```
