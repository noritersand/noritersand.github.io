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

날짜를 좀 편하게 다루려고 만든 프로토타입. 이름은 별 의미 ㅇ벗음.

가장 늦게 정해진 표준이 `static`인데 사파리 16에서 아직 구현이 안됐음. 참고할 것

## 코드

이렇게 씀:

```js
let now = new CoconutDate();
let yesterday = new CoconutDate({ dayPlus: -1 });
yesterday.of(); // Date Wed Jun 15 2022 20:13:51 GMT+0900 (대한민국 표준시)
yesterday.ofHighNoon(); // Date Wed Jun 15 2022 12:00:00 GMT+0900 (대한민국 표준시)
yesterday.dateString; // "2022-06-15"
yesterday.dateTimeString; // "2022-06-15 20:13:51"
yesterday.monthString; // "06" 
```

```js
class CoconutDate {
  static DATE_DELIMETER = "-";
  static TIME_DELIMETER = ":";
  #date;

  constructor({when = null, dayPlus = 0, monthPlus = 0, yearPlus = 0, format = ''} = {}) {
    this.#date = Boolean(when) ? new Date(when) : new Date();
    this.#date.setDate(this.#date.getDate() + dayPlus);
    this.#date.setMonth(this.#date.getMonth() + monthPlus);
    this.#date.setFullYear(this.#date.getFullYear() + yearPlus);
  }

  #toDateString() {
    // FIXME constructor의 format에 따라 다른 형식으로 변환 필요
    return this.#date.getFullYear() + CoconutDate.DATE_DELIMETER + (this.#date.getMonth() + 1).toString().padStart(2, '0')
        + CoconutDate.DATE_DELIMETER + this.#date.getDate().toString().padStart(2, '0');
  }
  #toDateTimeString() {
    // FIXME constructor의 format에 따라 다른 형식으로 변환 필요
    return this.#toDateString() + ' ' + this.#date.getHours().toString().padStart(2, '0')
      + CoconutDate.TIME_DELIMETER + this.#date.getMinutes().toString().padStart(2, '0')
      + CoconutDate.TIME_DELIMETER + this.#date.getSeconds().toString().padStart(2, '0');
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
    return new Date(`${this.#toDateString()}T00:00:00.000+09:00`);
  }

  /**
   * date와 같은 날이지만 12시에 해당하는 Date를 반환한다.
   * @returns {Date}
   */
  ofHighNoon() {
    return new Date(`${this.#toDateString()}T12:00:00.000+09:00`);
  }

  /**
   * #date를 년월일을 문자열로 반환
   * @returns {string}
   */
  get dateString() {
    return this.#toDateString();
  }

  /**
   * #date를 년월일+시분초를 문자열로 반환
   * @returns {string}
   */
  get dateTimeString() {
    return this.#toDateTimeString();
  }

  /**
   * 년도를 4자리 문자열로 반환
   * @returns {string}
   */
  get yearString() {
    return this.#date.getFullYear().toString();
  }

  /**
   * 월을 2자리 문자열로 반환
   * @returns {string}
   */
  get monthString() {
    return (this.#date.getMonth() + 1).toString().padStart(2, '0');
  }

  /**
   * 일자를 2자리 문자열로 반환
   * @returns {string}
   */
  get dayString() {
    return this.#date.getDate().toString().padStart(2, '0');
  }
}
```

끗.
