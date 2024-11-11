---
layout: post
date: 2023-09-10 17:44:38 +0900
title: '[TypeScript] 타입스크립트 기본'
categories:
  - typescript
tags:
  - typescript
  - language
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [TypeScript Documentation](https://www.typescriptlang.org/ko/docs/)

#### 테스트 환경 정보

- TypeScript 5.x.x


## 개요

Microsoft에서 만든 타입스크립트(TypeScript)의 기본적인 내용을 정리한 글.

타입스크립트는 이름처럼 정적 데이터 타입이 추가된 언어로, Node.js 환경에서 작동한다. 자바스크립트의 슈퍼셋(superset)이라 하기도 한다.

타입스크립트로 작성된 소스는 브라우저나 자바스크립트 엔진이 읽을 수 없기 때문에 컴파일을 통해 자바스크립트 코드로 변환된다. 이 때 정적 타입 검사기(static type checker)가 작동하며 문제를 발견하면 컴파일 에러를 발생시킨다.

타입스크립트의 주요 장점은 스크립트를 실행하기 전에 미리 문제를 발견할 수 있다는 점이다. 그러나 이 장점은 자바스크립트의 유연성을 제물로 바친 대가이며, 엄격한 타입 체크가 때로는 생산성을 떨어트리는 단점이 되기도 한다.


## 설치

```bash
npm install -g typescript
```

런타임이 Bun인 경우 따로 설치하지 않아도 됨.

```bash
# 버전 확인
tsc -v
tsc --version

# 도움말 보기
tsc -h
tsc --help
```


## 스캐폴딩: 프로젝트 기본 뼈대 만들기

CLI 명령으로 타입스크립트 프로젝트의 기본 구조를 생성하는 방법이다:

```bash
tsc --init
```

...라고 했지만 사실 현재 경로에 `tsconfig.json`을 만들어주는 게 끝.

### tsconfig.json

[tsconfig \| TypeScript Documentation ](https://www.typescriptlang.org/tsconfig)

타입스크립트 컴파일러가 프로젝트를 어떻게 컴파일 할지에 대한 설정을 정의한다. 저 아래 '빌드하기' 항목에서처럼 빌드할 항목 등을 직접 지정하는 방법도 있지만 그건 귀찮으니께...

#### tsconfig.json의 주요 설정:

- `target`: 컴파일될 자바스크립트 코드의 ECMAScript 버전을 지정한다. `es3`, `es5`, `es6`/`es2015`, `es2016`, `es2017`, `es2018`, `es2019`, `es2020`, ..., `esnext` 중에 하나이며 대소문자는 상관 없는 모양이다.
- `module`: 모듈 시스템을 지정한다. `none`, `commonjs`, `amd`, `umd`, `system`, `es6/es2015`, `es2020`, `es2022`, `esnext`, `node16`, `nodenext` 이것도 대소문자는 가리지 않는 걸로 보인다.
- `outDir`: 컴파일된 파일들이 저장된 디렉터리 경로
- `rootDir`: **TODO**
- `noImplicitAny`: (타입을 명시하지 않아 발생하는) 암묵적인 `any` 타입을 허용하지 않는다.
- `strictNullChecks`: 이 설정이 `true`면 `null`과 `undefined`를 다른 타입의 값으로 할당할 수 없다.
- `files`: **TODO**
- `include`: **TODO**
- `exclude`: **TODO**

`tsc --init`은 아래처럼 만들어줌:

```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```


## 빌드하기

```bash
tsc
tsc -b
tsc --build
```

만약 TypeScript 패키지를 전역으로 설치하지 않았고, `tsconfig.json`이 없다면?

```bash
# NPM: tsc 앞 뒤의 --는 outdir 옵션 처리를 위한 것
npm exec -- tsc -- .\src\test\tsc-me.ts --outdir ./dist

# Yarn
yarn run tsc .\src\test\tsc-me.ts --outdir ./dist
```

### 감시 모드로 빌드하기

파일의 변화를 감지해 자동으로 빌드한다:

```bash
tsc -b -w
tsc --build --watch
```


## 사용 가능한 타입 목록

- 원시 타입:
  - `string`: 문자열 타입
  - `number`: 숫자 타입 (정수, 실수 모두 포함)
  - `boolean`: 논리 타입 (true/false)
  - `symbol`: ES6에서 추가된 심볼 타입
  - `bigint`: ES2020에서 추가된 큰 정수 타입
- 배열:
  - `number[]`: 숫자 타입의 배열
  - `string[]`: 문자열 타입의 배열
  - ...
  - `Array<number>`: 제네릭 배열 타입 (이 경우 number 타입의 배열을 의미함)
- 튜플: 고정된 개수의 요소와 각 요소의 타입을 정확히 지정한 배열
- 열거형: 명명된 상수들의 집합을 정의하는 타입
- 사용자 정의 타입:
  - 타입 별칭: 복잡한 타입에 별칭을 붙여 사용하는 타입
  - 인터페이스: 객체 구조를 정의하는 타입
- 유니언: 여러 타입 중 하나일 수 있는 타입
- 인터섹션: 여러 타입을 모두 만족하는 타입
- 제네릭: 타입을 매개변수화하여 재사용 가능한 컴포넌트를 만드는 기능
- `any`: 어떠한 값도 할당 가능한 타입. 타입 검사를 비활성화한다.
- `null`과 `undefined`: 각각 `null` 값과 `undefined` 값을 나타내는 타입
- `void`: 함수에서 반환 값이 없을 때 사용하는 타입
- `never`: 절대 발생하지 않는 값의 타입 (예: 항상 예외를 throw하는 함수)
- 유틸리티 타입: 타입 변환용 편의성 타입

### 열거형 Enums

[Enums \| TypeScript Documentation](https://www.typescriptlang.org/ko/docs/handbook/enums.html)

**TODO**

### 타입 별칭 Type Aliases

`type` 키워드로 커스텀 타입을 정의한다. 객체나 유니언(아래에 따로 설명함)의 타입을 제한할 때 주로 사용한다:

```ts
function printCoordinate(obj: Coordinate) {
  return `${obj.latitude} ${obj.longitude}`;
}
printCoordinate({latitude: 0, longitude: 0}); // '0 0'
```

```ts
type MyNumber3 = number | string;
let num3: MyNumber3 = 10;
let num4: MyNumber3 = '10';
```

```ts
type Person = {
  name: string;
  age: number;
  email: string;
};

let obj: Person = {
  name: 'Waldo',
  age: 123,
  email: 'a@b.co'
};
```

ℹ️ 객체 구조 정의에는 타입 별칭보단 인터페이스를 사용하며, 타입 별칭은 함수, 유니온 등에 주로 사용한다.

### 인터페이스 Interfaces

타입 별칭과 매우 유사한데, 인터페이스는 확장(extends)이 가능하다는 차이가 있다:

```ts
interface Person {
  name: string;
  age: number;
}

interface Developer extends Person {
  skills: string[];
}

const person1: Person = {
  name: '김사람',
  age: 20
};

const person2: Developer = {
  name: '김개발',
  age: 20,
  skills: ['javascript', 'react', 'typescript']
};
```

ℹ️ 객체 타입을 정의할 땐 프로퍼티 구분자로 쉼표`,` 대신 세미콜론`;`이 권장된다. (둘 다 가능하긴 함)

#### 인터페이스의 프로퍼티를 타입 제한에 사용하기

```ts
interface SomeInterface {
  option: 'a' | 'b' | 'c';
}

function fn123(str: SomeInterface['option']) {
  console.log(str);
}
```

### 유니언 Unions

여러 타입 중 하나일 수 있음을 나타내는 방법이다. 허용할 타입들은 파이프`|`로 구분한다.

```ts
type MyNumber = number | string;
let n: MyNumber = 1;
n = '1'; // OK
n = false; // error TS2322: Type 'boolean' is not assignable to type 'MyNumber'.

// 배열 내부 요소에 적용
type MyNumber2 = 1 | 2 | 3 | '1' | '2' | '3';
let n2: MyNumber2 = 1;
n2 = '3'; // OK
n2 = 4; // error TS2322: Type '4' is not assignable to type 'MyNumber2'.
let arr2: MyNumber2[] = [1, '2', 3]; // OK

// 타입 별칭 없이 유니언
let arr3: (RegExp | string)[] = [/\d/, '-', /\d/];
```

```ts
type Person = {name: string; age: number};
type Human = {breathing: boolean};

let waldo: Person | Human;

waldo = {
  name: 'Waldo',
  age: 47,
  breathing: true
}; // OK

waldo = {
  breathing: true
}; // OK

waldo = {
  age: 12
}; // error TS2322: Type '{ age: number; }' is not assignable to type 'Person | Human'. Property 'name' is missing in type '{ age: number; }' but required in type 'Person'.

waldo = {
  hello: 'Hello there!'
}; // error TS2353: Object literal may only specify known properties, and 'hello' does not exist in type 'Person | Human'.
```

🚨 객체 타입끼리 유니언할 때 빈 객체를 지정하면 객체 프로퍼티 제한이 풀려버리는 현상이 있다:

```ts
type FullObject = {name: string; age: number};
type EmptyObject = {};

let whetherOrNot: FullObject | EmptyObject;

whetherOrNot = {
  name: 'Jack'
}; // OK
```

`{name: 'Jack'}` 값이 `FullObject` 타입에는 만족하지 않지만 `EmptyObject` 타입에 만족하기 때문.


### 인터섹션 Intersections

여러 타입 중 하나(OR)라도 만족하면 되는 유니언과 다르게 인터섹션은 여러 타입을 모두 만족(AND)해야 한다. 쉽게 설명하면 나열된 타입을 합친 새로운 타입을 만든다고 볼 수 있다. 객체 타입의 인터섹션은 객체의 프로퍼티를 병합하는데, 이 점은 인터페이스의 확장(Interface Extension)과 유사하다.

```ts
type Person = {name: string; age: number};
type Human = {breathing: boolean};

let waldo: Person & Human;

waldo = {
  name: 'waldo',
  age: 47,
  breathing: true
}; // OK

waldo = {
  name: 'waldo',
  age: 47
  // error TS2322: Type '{ name: string; age: number; }' is not assignable to type 'Person & Human'.
  // Property 'breathing' is missing in type '{ name: string; age: number; }' but required in type 'Human'.
};
```

인터섹션으로 서로 다른 원시 타입을 지정하면 실질적으로 사용할 수 없는 변수가 된다. 숫자이면서 동시에 문자열인 값은 존재할 수 없기 때문:

```ts
type Conflicting = number & string;

let foo1: Conflicting;
foo1 = '123'; // error TS2322: Type 'string' is not assignable to type 'never'.
foo1 = 123; // error TS2322: Type 'number' is not assignable to type 'never'.
```

### 제네릭 Generics

배열처럼 내부에서 별도로 처리하는 값들의 타입을 정의할 때 사용한다.

```ts
type StringArray = Array<string>;
let strArr: StringArray = ['a', 'b', 'c'];

type NumberArray = Array<number>;
let numArr: NumberArray = [1, 2, 3];
numArr.push('4'); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.

type ObjectWithNameArray = Array<{name: string}>;
let objArr: ObjectWithNameArray = [{name: '1'}, {name: '2'}];
```

아래는 공식 가이드의 예시를 약간 수정한 것:

```ts
interface Backpack<Type> {
  contents: Array<Type>;

  add: (obj: Type) => void;
  get: () => Array<Type>;
}

const backpack: Backpack<string> = {
  contents: [],

  add: function (obj) {
    this.contents.push(obj);
  },
  get: function () {
    return this.contents;
  }
};

const object = backpack.get();
console.log(object); // []

backpack.add('23'); // OK
console.log(object); // ['23']

backpack.add(23); // error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'.
```

아래는 함수 매개변수에 적용하는 방식이다. 이쪽은 *제네릭 타입 매개변수*라고 한다:

```ts
function identity<T>(arg: T): T {
  return arg;
}
```

### 유틸리티 타입 Utility Types

[Utility Types \| TypeScript: Documentation](https://www.typescriptlang.org/ko/docs/handbook/utility-types.html#recordkeystype)

타입스크립트에서 제공하는 타입 변환을 쉽게 하기 위한 유틸리티 타입이다. ~~넘모많다~~

- `Partial<Type>`
- `Required<Type>`
- `Readonly<Type>`
- `Record<Keys,Type>`: 
- `Pick<Type, Keys>`
- `Omit<Type, Keys>`
- `Exclude<Type, ExcludedUnion>`
- `Extract<Type, Union>`
- `NonNullable<Type>`
- `Parameters<Type>`
- `ConstructorParameters<Type>`
- `ReturnType<Type>`
- `InstanceType<Type>`
- `ThisParameterType<Type>`
- `OmitThisParameter<Type>`
- `ThisType<Type>`
- 내장 문자열 조작 타입
  - `Uppercase<StringType>`
  - `Lowercase<StringType>`
  - `Capitalize<StringType>`
  - `Uncapitalize<StringType>`


## 변수의 타입 제한

### 변수 타입 선언하기

```ts
let foo: number = 123;
```

위 코드는 `foo`가 `number` 타입임을 선언한다. 사실 타입스크립트는 변수에 할당된 값을 통해 *타입을 자동으로 추론*하기 때문에 변수에 대한 타입 표기는 선택 사항이다. (가독성을 위해 표기한다는 프로젝트 규칙이 있는 게 아니라면)

만약 선언한 타입과 다른 타입의 값을 할당하려 하면 타입 에러가 발생한다:

```ts
let foo2: string = 123; // error TS2322: Type 'number' is not assignable to type 'string'.
```

### 배열 Array

배열을 구성하는 요소의 타입을 제어하는 방식이다. 타입 뒤에 대괄호`[]`를 표기하여 선언한다:

```ts
let arr: number[];
arr = [1, 2, 3];
arr.push(4);
```

일반 원시 타입 값과 마찬가지로 선언한 타입과 할당값의 타입이 다르면 에러가 발생한다:

```ts
let arr: string[] = [1, 2, 3]; // error TS2322: Type 'number' is not assignable to type 'string'.
```

### 튜플 Tuple

튜플은 타입스크립트에 배열을 정의하는 방법 중 하나로, 배열의 고정된 개수의 요소와 각 요소의 타입을 정확히 지정할 때 사용한다.

```ts
let tuple: [string, number];
tuple = ['hello', 10];
```

함수 매개변수를 튜플로 선언하고 구조 분해 할당을 적용한 예시:

```ts
function calculateArea(dimensions: [number, number]): number {
  const [width, height] = dimensions;
  return width * height;
}

const area = calculateArea([3, 4]);
console.log(area); // 출력: 12
```

### 리터럴 타입 사용하기

타입스크립트에선 특이하게도 값의 범위도 지정할 수 있는데, 이것도 타입 체크의 범주로 포함하며 *리터럴 타입* 또는 *리터럴 집합*이라 한다:

```ts
let one: 1;
one = 1;
one = 2; // error TS2322: Type '2' is not assignable to type '1'.
```

변수 `one`에 대한 타입으로 리터럴 `1`을 명시했기 때문에 `1` 외에는 할당할 수 없는 변수가 된다.

### 객체 리터럴의 프로퍼티

식별자명 바로 뒤에 콜론`:`과 함께 어떤 타입인지를 선언한다:

```ts
let obj: {name: string} = {
  name: 'John'
};

console.log(obj.name); // John
```

하지만 명시하지 않은 객체의 프로퍼티에 접근하려 하면 컴파일 에러가 발생하는데:

```ts
let obj: {name: string} = {
  name: 'John'
};

console.log(obj.sirname); // error TS2339: Property 'sirname' does not exist on type '{ name: string; }'.
```

특정 프로퍼티가 있을 수도 없을 수도 있다면(이것은 객체 프로퍼티로 `undefined` 할당을 허용하는 것과 같다) 콜론에 물음표를 붙여서 `?:` *옵셔널 프로퍼티(Optional properties)*로 지정한다:

```ts
let obj: {name: string; sirname?: string} = {
  name: 'John'
};

console.log(obj.sirname); // undefined
```

### 프로퍼티 이름의 타입 정의: 인덱스 시그니처 Index Signatures

아래처럼 대괄호 표기법을 이용한 동적 프로퍼티 접근 코드는 에러를 유발하는데:

```js
let obj = {
  a: 1,
  b: 2
};

let b = 'b';
console.log(obj[b]);
// error TS7053: Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{ a: number; b: number; c: number; }'.
//   No index signature with a parameter of type 'string' was found on type '{ a: number; b: number; c: number; }'.
```

이럴 땐 아래처럼 [인덱스 시그니처](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)로 프로퍼티 이름과 값의 타입을 정의하여 해소할 수 있다:

```ts
let obj: {[key: string]: number} = {
  a: 1,
  b: 2
};

let b = 'b';
console.log(obj[b]); // 2
```

### 타입 단언 Type Assertions

컴파일러에게 특정 값의 타입을 명시적으로 알려주는 방법이다. `as` 혹은 홑화살괄호`<>`로 표기한다:

```ts
let myCanvas = document.querySelector('#myCanvas') as HTMLCanvasElement;

let myCanvas2 = <HTMLCanvasElement>document.querySelector('#myCanvas');
```

**어떤 값이 올 수 있는 자리에 대한 타입을 '미리' 선언하는 것이 아니라, 이미 존재하는 값의 타입이 어떤 것인지를 개발자가 컴파일러에게 '나중에' 알려준다는 차이**가 있다.

🚨 타입 단언은 타입 체커를 무시하며 코드 안정성을 보장하지 않는다. 가급적이면 타입 단언 대신 타입 가드나 타입 체크를 사용하는 것이 권장된다.

#### const-assertion

*const-assertion*은 타입 단언의 일종으로, 배열과 객체 리터럴의 타입 추론을 개선하고 반환된 값을 읽기 전용으로 만드는 특수한 문법이다. 리터럴 뒤에 `as const` 키워드를 붙여 선언한다:

```ts
let a = [1, 2, 3] as const;
a.push(102); // error
a[0] = 101; // error
```

**TODO** 설명 추가


## 함수의 타입 제한

이 쪽은 호출 시그니처(call signatures)라고 한다. 호출 시그니처는 함수나 메서드의 타입을 설명하는 방법으로, 매개변수의 타입과 반환값의 타입을 정의할 때 사용한다.

### 매개변수의 타입 선언

```ts
function add(a: number, b: number) {
  // ...
}
```

매개변수 `a`와 `b` 모두 `number` 타입이라는 뜻이다.

#### 함수 매개변수 기본값(default function parameters)과 같이 설정하기

```ts
function getNotice(noticeNo: number = null) {
  // ...
}
```

### 객체 타입의 매개변수

얼핏 보면 구조분해 처럼 보이지만 사실은 객체 프로퍼티에 대한 타입을 표기한 것이다. 각 프로퍼티를 구분할 땐 쉼표`,` 혹은 세미콜론`;`을 사용한다.

```ts
function printFooBar(foobar: {foo: string; bar: number}) {
  // ...
}

function printFooBar2(foobar: {foo: string; bar: number}) {
  console.log(foobar.foo, foobar.bar);
}
printFooBar2({foo: 'foo', bar: 1}); // foo 1
```

하지만 이렇게 객체 프로퍼티에 대한 타입을 지정하면 해당 프로퍼티를 생략했을 때 타입 에러가 발생한다:

```ts
function printFooBar2(foobar: {foo: string; bar: number}) {
  // ...
}
printFooBar2({foo: 'foo'});
// error TS2345: Argument of type '{ foo: string; }' is not assignable to parameter of type '{ foo: string; bar: number; }'. Property 'bar' is missing in type '{ foo: string; }' but required in type '{ foo: string; bar: number; }'.
```

객체 리터럴에서처럼 `undefined` 할당을 허용하는 *옵셔널 프로퍼티*로 만들려면 물음표`?`를 붙인다:

```ts
function printFooBar3(foobar: {foo: string; bar?: number}) {
  // ...
}
printFooBar3({foo: 'foo'});
```

#### 함수 매개변수 기본값과 같이 설정하기

```ts
function addAtoB({a = 0, b = 1}: {a: number; b: number}) {
  // ...
}
```

### 반환값의 타입 선언

다른 것과 비슷하게 콜론`:`을 사용한다:

```ts
// 함수 정의식
function getSymbol1(): symbol {
  return Symbol('me');
}

// 화살표 함수
const getSymbol2 = (): symbol => {
  return Symbol('me too');
};
```

만약 문맥 상 반환 타입을 예측할 수 있다면 생략 가능하다.

### 함수 타입 표현식 Function Type Expressions

함수 타입 매개변수(콜백 함수)에 대한 타입 표기 방법이다. 여기선 반환 타입 표기에 화살표`=>`를 사용한다:

```ts
function caller(callee: (a: string) => void) {
  callee('hello');
}
```

다른 예로 콜백 함수의 매개변수가 없고 반환 타입이 `string`이면 아래처럼 표기한다:

```ts
function caller2(callee: () => string) {
  return callee();
}
console.log(caller2(() => 'hello')); // hello
```

### 익명 함수

익명 함수(화살표 함수 포함)의 타입 표기 생략은 문맥에 따라 결정된다:

```ts
let names = ['a', 'b', 'c'];
names.forEach(s => console.log(s.toUpperCase()));
```

```ts
let arr2 = [];
arr2.forEach(n => console.log(n.toFixed(2)));
// error TS7034: Variable 'arr2' implicitly has type 'any[]' in some locations where its type cannot be determined.
```

위 코드는 이렇게 바꿔야 한다:

```ts
let arr2: number[] = [];
arr2.forEach(n => console.log(n.toFixed(2)));
```

이렇게 자동으로 타입을 알아내는 것은 *문맥적 타입 부여(contextual typing)*라고 한다(*타입 추론*과는 별개의 개념이다).

익명 함수의 반환 타입은 이렇게 작성한다:

```ts
let getWaldo = (): string => 'Waldo';
getWaldo().toUpperCase();
```

이 또한 문맥 상 자동으로 알 수 있는 상황이라면 생략할 수 있긴 하다. 하지만 아래의 경우라면:

```ts
let getWaldo = (name: string) => {
  return {name};
};
getWaldo('Waldo').age = 128;
// error TS2339: Property 'age' does not exist on type '{ name: string; }'.
```

`getWaldo()`가 반환하는 객체의 프로퍼티로 `age`가 명시되지 않았기 때문에 에러가 발생한다. 이를 해결하려면:

```ts
let getWaldo = (name: string): {name: string; age?: number} => {
  return {name};
};
getWaldo('Waldo').age = 128;
```

### 생성자 시그니처 Construct Signatures

[https://www.typescriptlang.org/docs/handbook/2/functions.html#construct-signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#construct-signatures)

**TODO**


## 클래스(Classes)의 형태 제한

클래스에서의 타입 사용은 앞서 언급했던 것들의 조합이다:

```ts
class Newbie {
  name: string; // 프로퍼티 타입 표기
  id: number;

  constructor(name: string, id: number) {
    // 함수 매개변수 타입 표기
    this.name = name;
    this.id = id;
  }
}
```

### private

필드를 클래스 내부에서만 접근할 수 있는 비공개 필드로 만드는 기능이다. ECMAScript의 `#` 키워드를 붙이는 것과 같다(이 문법도 지원한다).

**TODO**

### public

**TODO**

### readonly

필드를 읽기 전용으로 만들어 생성자 밖에서 재할당을 할 수 없도록 하는 기능이다.

**TODO**

### 구현/구상화 implements

타입 스크립트의 인터페이스도 자바의 인터페이스와 유사하게 클래스의 구상화 기능을 제공한다:

```ts
// 코드 출처: https://www.typescriptlang.org/docs/handbook/2/classes.html#class-heritage
interface Pingable {
  ping(): void;
}

class Sonar implements Pingable {
  ping() {
    console.log('ping!');
  }
}
```

`ping()`은 인터페이스 내에 선언된 메서드 시그니처(method signatures)라고 한다. 이것은 구현 클래스에 `ping()`이 반드시 있도록 제한한다:

```ts
class Ball implements Pingable {
  // error TS2420: Class 'Ball' incorrectly implements interface 'Pingable'.
  //   Property 'ping' is missing in type 'Ball' but required in type 'Pingable'.
  // ping() 메서드를 구현하지 않은 경우
}
```

⚠️ `implements`는 클래스나 메서드의 타입을 변경하지 않는다(_It doesn’t change the type of the class or its methods at all_: 타입 선언을 의미하는 것 같음). 이것은 인터페이스에 정의된 메서드의 매개변수 타입이 자동으로 구현 클래스에 적용되지 않는다는 말이다:

```ts
interface Checkable {
  check(name: string): boolean;
}

class NameChecker implements Checkable {
  check(s): boolean {
    // error TS7006: Parameter 's' implicitly has an 'any' type.
    return false;
  }
}
```

### 추상 클래스 Abstract Classes

추상 클래스는 인스턴스 생성이 불가능하며 상속(extends)을 위해서면 존재하는 클래스다. 다른 클래스로 파생되는 기반 클래스 역할을 하며, 구현 클래스의 형태를 제한한다.

아래는 추상 메서드 `getName()`를 갖는 추상 클래스를 선언하는 방법이다:

```ts
// 코드 출처: https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members
abstract class Base {
  abstract getName(): string;

  printName() {
    console.log('Hello, ' + this.getName());
  }
}

class Derived extends Base {
  getName() {
    return 'world';
  }
}
```

이제 `Derived`는 `printName()` 메서드를 상속 받으며:

```js
const d = new Derived();
d.printName(); // Hello, world
```

만약 `getName()` 메서드를 구현하지 않으면 에러가 발생한다:

```ts
class Derived extends Base {} // error TS18052: Non-abstract class 'Derived' does not implement all abstract members of 'Base'
```

### 추상 생성자 시그니처 Abstract Construct Signatures

타입스크립트에선 매개변수에 대한 타입으로 생성자를 선언할 수 있다. 이것은 그 중 추상 클래스의 생성자 타입을 제한하는 방법이다. (별 게 다 있다 🥲)

```ts
abstract class Base {
  abstract printName(): void;
}

class Derived extends Base {
  printName() {
    console.log('Derived instance');
  }
}

function greet(ctor: new () => Base) {
  const instance = new ctor();
  instance.printName();
}

greet(Derived);
```

추상 클래스 `Base`와 `Base`를 상속하는 `Derived`가 있을 때, `greet()` 함수에서 `Base`와 `Base`의 서브 클래스 생성자를 매개변수로 받도록 타입을 제한했다. 그리고 `Derived` 생성자 함수를 인수로 넘기는 코드다.

만약 `Base` 생성자 함수를 인수로 하면:

```ts
greet(Base);
// error TS2345: Argument of type 'typeof Base' is not assignable to parameter of type 'new () => Base'.
// Cannot assign an abstract constructor type to a non-abstract constructor type.
```

추상 클래스는 인스턴스를 만들 수 없기 때문에 에러가 발생한다.

🚨 `typeof` 타입 연산자로도 비슷한 구현이 가능하지만 권장되지 않는다.


## 구조적 타입 시스템 Structural Type System

변수의 타입 선언을 생략해도 형태를 비교하여 타입을 판단하는 특성을 말한다. 아래 공식 가이드의 코드를 보면:

```ts
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

const point = {x: 12, y: 26};
printPoint(point);
```

`point`는 `Point` 타입으로 선언된 적이 없지만, 타입 검사기가 알아서 형태를 비교하고 통과시킨다. 따라서 이 코드는 정상이다.

형태를 비교할 때는 부분 집합인지만 통과하면 된다.

```ts
const point2 = {x: 12, y: 26, z: 89}; // OK
const rect = {x: 33, y: 3, width: 30, height: 80}; // OK
```

하지만 전혀 엉뚱한 구조라면:

```ts
const color = {hex: '#187ABF'};
// error TS2345: Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
// Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

반기는 것은 타입 에러다.


## 앰비언트 선언 Ambient Declarations

`declare` 키워드로 변수나 모듈, 함수, 클래스 등의 존재를 타입스크립트에 알리기 위해 사용하는 선언 방식.

```ts
declare var myVariable: string;
declare function myFn(a: number): void;
declare class MyClass {}
```

앰비언트 선언은 보통 `.d.ts` 파일(타입스크립트 선언 파일)에 작성한다. 이 파일들은 컴파일 중 코드로 변환되지 않으며, 오직 타입 체크와 자동 완성, 문서화를 위해 사용된다.

**TODO** 설명 추가

[Creating .d.ts Files from .js files \| TypeScript Documentation](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)


## 다운레벨링 Downleveling

다운레벨링은 상위 버전의 스크립트를 하위 버전으로 바꿔주는 기능을 의미하는 용어다.

아래처럼 ES2015에 도입된 기술은 템플릿 리터럴을 사용한 코드는:

```ts
let person = 'John Doe';
`Hello ${person}.`;
```

`tsconfig.json`의 `target` 설정이 ES2015보다 낮은 ES5라면 아래처럼 컴파일된다:

```js
'use strict';
var person = 'John Doe';
'Hello '.concat(person, '.');
```

하지만 다운레벨링이 모든 것을 가능하게 해주는 요술봉은 아니다. 예를 들어 ES5에서 `symbol` 타입은 사용할 수 없기 때문에 컴파일 에러가 발생한다:

```ts
// "target": "es5"
let s: symbol = Symbol('s');
// error TS2585: 'Symbol' only refers to a type, but is being used as a value here. Do you need to change your target library? Try changing the 'lib' compiler option to es2015 or later.
```


## 연산자

### Keyof 타입 연산자

[Keyof Type Operator \| TypeScript Documentation](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

`keyof`는 타입 별칭이나 인터페이스에 정의된 모든 프로퍼티들의 이름을 문자열 리터럴 유니언으로 반환하는 연산자다. 보통 객체의 프로퍼티 이름을 추출하여 타입으로 사용할 때 사용한다:

```ts
type AType = {
  a: string;
  b: number;
};

interface BType {
  c: boolean;
  d: string[];
}

let foo3: keyof AType;
foo3 = 'a';
foo3 = 'b';
// foo3 = 'c'; // error TS2322: Type '"c"' is not assignable to type 'keyof AType'.

let foo4: keyof BType;
foo4 = 'c';
foo4 = 'd';
// foo4 = 'g'; // Type '"g"' is not assignable to type 'keyof BType'.
```

그러니까 `keyof AType`은 `'a' | 'b'`, `keyof BType`은 `'c' | 'd'`가 되는 식이다. 자바스크립트의 `Object.keys()`와 유사하지만, 런타임에는 존재하지 않는 문법이므로 출력에 직접 사용할 수는 없다:

```ts
console.log(keyof Atype); // error TS1005: ',' expected.
```

아래는 제네릭 타입에 활용한 예시다:

```ts
type Person3 = {
  name: string;
  age: number;
  city: string;
};

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person3: Person3 = {
  name: 'John',
  age: 30,
  city: 'New York'
};

getProperty(person3, 'name'); // John
getProperty(person3, 'age'); // 30
getProperty(person3, 'city'); // New York
```

제네릭 타입 `T`와 `K`를 사용하며, `K`의 타입을 `keyof T`로 제한했다. 이는 `K`가 `T` 타입의 프로퍼티 이름 중 하나여야 함을 의미한다.

### Typeof 타입 연산자

[Typeof Type Operator \| TypeScript Documentation](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

**TODO**


## 타입스크립트의 JSDoc

[JSDoc Reference \| TypeScript Documentation](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)

**TODO**
