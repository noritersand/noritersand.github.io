---
layout: post
date: 2022-06-16 14:27:19 +0900
title: '[JavaScript] CoconutDate'
categories:
  - javascript
tags:
  - javascript
  - date
  - prototype
  - class
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 버전 정보

- IE, 사파리(16)에서 사용 불가


## 개요

날짜를 좀 편하게 다루려고 만든 프로토타입. 이름은 별 의미 ㅇ벗다.

이 코드에서 사용한 것 중 가장 늦게 정해진 표준이 `static`인데 사파리 16에서 아직 구현이 안됐음.


## 코드

이렇게 씀:

```js
let now = new CoconutDate();
let yesterday = new CoconutDate({ dayPlus: -1 });

now.toDateString(); // 2023-01-19 
now.toDateTimeString(); // 2023-01-19 14:28:45.264 

yesterday.of(); // Date Wed Jan 18 2023 14:28:45 GMT+0900 (대한민국 표준시)
yesterday.ofHighNoon(); // Date Wed Jan 18 2023 12:00:00 GMT+0900 (대한민국 표준시)
yesterday.ofMidnight(); // Date Wed Jan 18 2023 00:00:00 GMT+0900 (대한민국 표준시)

yesterday.getYearOfEra(); // 2023 
yesterday.getMonthOfYear(); // 01 
yesterday.getDayOfMonth(); // 18 
```

```js
class CoconutDate {
  static DATE_DELIMITER = "-";
  static TIME_DELIMITER = ":";
  static MILLISECOND_DELIMITER = ".";
  #date;

  constructor({when = null, dayPlus = 0, monthPlus = 0, yearPlus = 0, hourPlus = 0, minutePlus = 0, secondPlus = 0, millisecondPlus = 0, format = ''} = {}) {
    this.#date = Boolean(when) ? new Date(when) : new Date();
    dayPlus && this.#date.setDate(this.#date.getDate() + dayPlus);
    monthPlus && this.#date.setMonth(this.#date.getMonth() + monthPlus);
    yearPlus && this.#date.setFullYear(this.#date.getFullYear() + yearPlus);
    hourPlus && this.#date.setHours(this.#date.getHours() + hourPlus);
    minutePlus && this.#date.setMinutes(this.#date.getMinutes() + minutePlus);
    secondPlus && this.#date.setSeconds(this.#date.getSeconds() + secondPlus);
    millisecondPlus && this.#date.setMilliseconds(this.#date.getMilliseconds() + millisecondPlus);
  }

  toDateString() {
    // FIXME constructor의 format에 따라 다른 형식으로 변환 필요
    return this.#date.getFullYear() + CoconutDate.DATE_DELIMITER + (this.#date.getMonth() + 1).toString().padStart(2, '0')
        + CoconutDate.DATE_DELIMITER + this.#date.getDate().toString().padStart(2, '0');
  }

  toDateTimeString() {
    // FIXME constructor의 format에 따라 다른 형식으로 변환 필요
    return this.toDateString() + ' ' + this.#date.getHours().toString().padStart(2, '0')
      + CoconutDate.TIME_DELIMITER + this.#date.getMinutes().toString().padStart(2, '0')
      + CoconutDate.TIME_DELIMITER + this.#date.getSeconds().toString().padStart(2, '0')
      + CoconutDate.MILLISECOND_DELIMITER + this.#date.getMilliseconds().toString().padStart(3, '0');
  }

  /**
   * Date 인스턴스 반환
   * @returns {Date}
   */
  of() {
    return this.#date;
  }

  /**
   * date와 같은 날이지만 00시에 해당하는 Date를 반환한다.
   * @returns {Date}
   */
  ofMidnight() {
    return new Date(`${this.toDateString()}T00:00:00.000+09:00`);
  }

  /**
   * date와 같은 날이지만 12시에 해당하는 Date를 반환한다.
   * @returns {Date}
   */
  ofHighNoon() {
    return new Date(`${this.toDateString()}T12:00:00.000+09:00`);
  }

  /**
   * 년도를 4자리 문자열로 반환
   * @returns {string}
   */
  getYearOfEra() {
    return this.#date.getFullYear().toString();
  }

  /**
   * 월을 2자리 문자열로 반환
   * @returns {string}
   */
  getMonthOfYear() {
    return (this.#date.getMonth() + 1).toString().padStart(2, '0');
  }

  /**
   * 일자를 2자리 문자열로 반환
   * @returns {string}
   */
  getDayOfMonth() {
    return this.#date.getDate().toString().padStart(2, '0');
  }
}
```

원래 `getter`가 잔뜩 있었는데, 아무래도 `getter`는 필드를 단순 반환만 하는게 맞는 것 같아서 없앰.

끗.
