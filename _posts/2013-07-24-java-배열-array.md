---
layout: post
date: 2013-07-24 00:27:00 +0900
title: '[Java] 배열 array'
categories:
  - java
tags:
  - java
  - array
---

* Kramdown table of contents
{:toc .toc}

자바에서 배열을 다루는 방법을 간단히 기술한 글.

## 배열의 선언과 생성

```
자료형[] 변수명;  // 권장
자료형 변수명[];
```

```java
int[] ns;
int ns[];

int[] score; // 배열 선언
score = new int[5]; // 배열 생성

int[] score = new int[5]; // 배열의 선언과 생성
```

`score = new int[5]` 는 다음과 같은 순서로 처리된다:

1. 연산자 'new'에 의해 메모리에 5개의 int형 데이터 저장공간이 생성된다.
1. 각 배열요소는 자동적으로 int의 기본값인 0으로 초기화 된다.
1. 배열의 주소가 int형 배열 참조변수 score에 저장된다.

#### 변수의 타입에 따른 초기화 기본값

| 자료형      | 기본값        |
|-------------|---------------|
| boolean     | false         |
| char        | `'\u000'`       |
| byte        | 0             |
| short       | 0             |
| int         | 0             |
| long        | 0L            |
| float       | 0.0f          |
| double      | 0.0d 또는 0.0 |
| 참조형 변수 | null          |

배열의 선언은 저장공간의 생성이 아닌 배열을 다루기 위한 변수생성을 의미한다.

## 배열의 초기화

배열의 초기화란 특정 값으로의 초기화 혹은 특정 값의 할당을 의미한다.

```
// 배열 선언 + 생성
자료형[] 변수명 = new 자료형[크기];

// 배열 초기화
변수명[0] = 값1;
변수명[1] = 값2;

// 배열 선언 + 생성 + 초기화
자료형[] 변수명 = { 값1, 값2, 값3... };
자료형[] 변수명 = new 자료형[] { 값1, 값2, 값3... };  
자료형[] 변수명 = { 값1, 값2, 값3... };  
```

```java
int[] score;
score = { 1, 2, 3, 4, 5 };   // 컴파일 에러 -> Array constants can only be used in initializers

int[] score;
score = new int[] { 1, 2, 3, 4, 5 }; //correct
int[] score = { 1, 2, 3, 4, 5 }; //correct
int[] score = new int[] { 1, 2, 3, 4, 5 }; //correct
```

### 다차원 배열의 초기화

```
// 생성과 선언
자료형[][] 변수명 = new 자료형[n1][n2];
자료형[][][] 변수명 = new 자료형[n1][n2][n3];

// 초기화
자료형[][] 변수명 = { { 값1-1, 값1-2... }, { 값2-1, 값2-2... } };
자료형[][] 변수명 = new 자료형[][] { { 값1-1, 값1-2... }, { 값2-1, 값2-2... } };
```

```java
int[][] ns = new int[][] { { 1, 2, 3 }, { 4, 5, 6 } };

// 위는 아래와 같다.

int[][] ns = new int[2][3];
ns[0][0] = 1;
ns[0][1] = 2;
ns[0][2] = 3;
ns[1][0] = 4;
ns[1][1] = 5;
ns[1][2] = 6;
```

만약 다음처럼 다차원 배열을 생성했을때:

```java
int[][] ns = new int[5][3];
```

여기서 `ns.length`는 가장 바깥에 있는 배열의 길이인 5가 된다.

### 가변 배열

자바의 다차원 배열은 '배열의 배열' 형태로 처리한다는 점을 이용하여 전체 배열 차수 중 마지막 차수의 크기를 지정하지 않고 추후에 각기 다른 크기의 배열을 생성함으로써 고정된 형태가 아닌 보다 유동적인 가변 배열을 구성할 수 있다.

```java
int[][] score = new int[3][];   // 두 번째 차원의 크기를 지정하지 않는다.

score[0] = new int[2];  // 첫 번째 차원의 배열을 통해 각각 다른 길이의 배열을 선언하거나 선언하지 않을 수도 있다.
score[1] = new int[3];
//score[2]는 초기화하지 않았다.

score; // [[0, 0], [0, 0, 0], null]
```

주의할 것이 있는데, 만약 다음처럼 다차원 배열을 초기화하면:

```java
int[][] ns = new int[][] { { 1, 2, 3 } };
```

위는 가변배열이 아니라 ns[0]에 길이 3의 배열을 초기화 하는것이다. 즉, 아래와 같다:

```java
int[][] ns = new int[0][3];
ns[0][0] = 1;
ns[0][1] = 2;
ns[0][2] = 3;

ns; // [[1, 2, 3]]
```

### example

```java
public class ArrayEx1 {
    public static void main(String[] args) {
        int sum = 0;
        float avg = 0f;

        int[] score = { 100, 88, 100, 100, 90 };

        for (int loop=0; loop<score; loop++) {
            sum += score[loop];  // 반복문을 이용, 배열에 저장된 값의 총합을 구함
        }

        avg = sum / (float)score.length;

        System.out.println("총점 : " + sum);
        System.out.println("평균 : " + avg);
    }
}
```

## 배열의 복사

### for문을 이용한 복사

```java
int[] number = { 1, 2, 3, 4, 5 };
int[] newNumber = new int[number.length];

for (int loop = 0; loop < number.length; loop++) {
	newNumber[loop] = number[loop];
}
```

### System.arraycopy() 를 이용한 복사

```java
int[] number = { 1, 2, 3, 4, 5 };
int[] newNumber = new int[number.length];

System.arraycopy(number, 0, newNumber, 0, number.length);
//number[0]에서 newNumber[0]으로 number.length개의 데이터를 복사
```

### `Arrays.copyOf()`를 이용한 복사

```java
int[] number = { 1, 2, 3, 4, 5 };
int[] newNumber;

newNumber = Arrays.copyOf(number, number.length);
```
