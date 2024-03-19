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

Microsoft에서 만든 TypeScript(이하 타입스크립트)의 기본적인 내용을 정리한 글.

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

[TypeScript Documentation \| tsconfig](https://www.typescriptlang.org/tsconfig)

타입스크립트 컴파일러가 프로젝트를 어떻게 컴파일 할지에 대한 설정을 정의한다. 저 아래 '빌드하기' 항목에서처럼 빌드할 항목 등을 직접 지정하는 방법도 있지만 그건 귀찮으니께...

#### tsconfig.json의 주요 설정:

- `target`: 컴파일될 자바스크립트 코드의 ECMAScript 버전을 지정한다. `es3`, `es5`, `es6/es2015`, `es2016`, `es2017`, `es2018`, `es2019`, `es2020`, `es2021`, `es2022`, `esnext` 중에 하나이며 대소문자는 상관 없는 모양이다.
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


## 타입 목록

- 원시 타입:
  - `string`
  - `number`
  - `boolean`
  - `symbol`
  - `bigint`
- 배열: 배열의 원소가 될 타입에 대괄호`[]`를 붙이는 방식으로 표기한다.
  - `number[]`
  - `string[]`
  - ...
- `any`: 어떠한 것도 할당 가능한 타입이다. 이 타입은 타입 검사를 비활성한다.
- Enums
- 커스텀 타입:
  - 유니언
  - 인터섹션
  - 제네릭


## 타입 정의(definition)와 선언(declaration)

### 변수

```ts
let foo: number = 123;
```

위 코드는 `foo`가 `number` 타입임을 선언한다. 사실 타입스크립트는 변수에 할당된 값을 통해 *타입을 자동으로 추론*하기 때문에 변수에 대한 타입 표기는 선택 사항이다. (가독성을 위해 표기한다는 프로젝트 규칙이 있는 게 아니라면)

당연하지만, 선언한 타입과 다른 타입의 값을 할당하려 하면 타입 에러가 발생한다.

```ts
let arr: string[] = [1, 2, 3]; // error TS2322: Type 'number' is not assignable to type 'string'.
```

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

특정 프로퍼티가 있을 수도 없을 수도 있다면(이것은 객체 프로퍼티로 `undefined` 할당을 허용하는 것과 같다) 물음표`?`를 붙여서 *옵셔널 프로퍼티(Optional properties)*로 지정한다:

```ts
let obj: {name: string, sirname?: string} = {
  name: 'John'
};

console.log(obj.sirname); // undefined
```

#### 프로퍼티 이름의 타입 정의 Index Signatures

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

다음처럼 [TypeScript Documentation \| Index Signatures](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)로 프로퍼티 이름과 값의 타입을 정의하여 해소할 수 있다:

```ts
let obj: {[key: string]: number} = {
  a: 1,
  b: 2
};

let b = 'b';
console.log(obj[b]); // 2
```

### 함수 매개변수

```ts
function add(a: number, b: number) {
  // ...
}
```

파라미터 `a`와 `b` 모두 `number` 타입이라는 뜻이다.

### 객체 타입의 매개변수

자바스크립트에서는 불가능한 문법인데, 언뜻 구조분해 처럼 보이지만 사실은 객체 프로퍼티에 대한 타입을 표기한 것이다. 각 프로퍼티를 구분할 땐 쉼표`,` 혹은 세미 콜론`;`을 사용한다.

```ts
function printFooBar(foobar: { foo: string; bar: number }) {
  // ...
}

function printFooBar2(foobar: { foo: string, bar: number }) {
  console.log(foobar.foo, foobar.bar);
}
printFooBar2({ foo: 'foo', bar: 1 }); // foo 1
```

하지만 이렇게 객체 프로퍼티에 대한 타입을 지정하면 해당 프로퍼티를 생략했을 때 타입 에러가 발생한다:

```ts
function printFooBar2(foobar: { foo: string, bar: number }) {
  // ...
}
printFooBar2({ foo: 'foo' });
// error TS2345: Argument of type '{ foo: string; }' is not assignable to parameter of type '{ foo: string; bar: number; }'. Property 'bar' is missing in type '{ foo: string; }' but required in type '{ foo: string; bar: number; }'.
```

객체 리터럴에서처럼 `undefined` 할당을 허용하는 *옵셔널 프로퍼티*로 만들려면 물음표`?`를 붙인다:

```ts
function printFooBar3(foobar: { foo: string, bar?: number }) {
  // ...
}
printFooBar3({ foo: 'foo' });
```

### 함수 반환 타입

```ts
function getSymbol(): symbol {
  return Symbol('me');
}
```

만약 문맥 상 반환 타입을 예측할 수 있다면 생략할 수 있다.


### 함수 타입 표현식 Function Type Expressions

함수 타입 파라미터에 대한 타입 표기 방법이다:

```ts
function caller(callee: (a: string) => void) {
  callee('hello');
}
```

콜백 함수의 매개변수가 없고 반환 타입이 `string`이면 아래처럼 표기한다:

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

여기서 자동으로 타입을 알아내는 것은 *타입 추론*이 아니라 *문맥적 타입 부여(contextual typing)*라고 한다.

익명 함수의 반환 타입은 이렇게 작성한다:

```ts
let getWaldo = (): string => 'Waldo';
getWaldo().toUpperCase();
```

이 또한 문맥 상 자동으로 알 수 있는 상황이라면 생략할 수 있긴 하다. 하지만 아래의 경우라면:

```ts
let getWaldo = (name: string) => {
  return {name}
};
getWaldo('Waldo').age = 128;
// error TS2339: Property 'age' does not exist on type '{ name: string; }'.
```

`getWaldo()`가 반환하는 객체의 프로퍼티로 `age`가 명시되지 않았기 때문에 에러가 발생한다. 이를 해결하려면:

```ts
let getWaldo = (name: string): {name: string, age?: number} => {
  return {name}
};
getWaldo('Waldo').age = 128;
```

### 클래스 Classes 

클래스에서의 타입 표기는 앞서 언급했던 것들의 조합이다:

```ts
class Newbie {
  name: string; // 프로퍼티 타입 표기
  id: number;

  constructor(name: string, id: number) { // 함수 매개변수 타입 표기
    this.name = name;
    this.id = id;
  }
}
```

### 리터럴 타입

타입스크립트에선 특이하게도 값의 범위(공식 가이드에선 이를 리터럴 집합이라 함)도 지정할 수 있는데, 이것도 타입 체크의 범주로 포함하며 *리터럴 타입*이라 한다:

```ts
let one: 1;
one = 1;
one = 2; // error TS2322: Type '2' is not assignable to type '1'.
```

변수 `one`에 대한 타입으로 리터럴 `1`을 명시했기 때문에 `1` 외에는 할당할 수 없는 변수가 된다.

### 타입 단언 Type Assertions

객체 타입을 좀 더 구체적으로 명시할 때 사용한다. `as` 혹은 홑화살괄호`<>`로 표기한다:

```ts
let myCanvas = document.querySelector('#myCanvas') as HTMLCanvasElement;

let myCanvas2 = <HTMLCanvasElement>document.querySelector('#myCanvas');
```


## 타입 별칭 Type Aliases

`type` 키워드로 정의한다. 객체 타입이나 유니언(아래에 따로 설명함)에서 주로 사용한다:

```ts
function printCoordinate(obj: Coordinate) {
  return `${obj.latitude} ${obj.longitude}`
}
printCoordinate({ latitude: 0, longitude: 0 }) // '0 0'
```

```ts
type myNumber3 = number | string;
let num3: myNumber3 = 10;
let num4: myNumber3 = '10';
```

```ts
type person = {
  name: string;
  age: number;
  email: string;
}

let obj: person = {
  name: "Waldo",
  age: 123,
  email: "a@b.co"
}
```


## 인터페이스 Interfaces

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

### Class Heritage

**TODO** 설명 추가

```ts
// 코드 출처: https://www.typescriptlang.org/docs/handbook/2/classes.html#class-heritage
interface Pingable {
  ping(): void;
}
 
class Sonar implements Pingable {
  ping() {
    console.log("ping!");
  }
}
```

```ts
class Ball implements Pingable {
// Class 'Ball' incorrectly implements interface 'Pingable'.
//   Property 'ping' is missing in type 'Ball' but required in type 'Pingable'.
  
  // ping() 메서드를 구현하지 않은 경우
}
```


## 추상 클래스 Abstract Classes

추상 클래스는 인스턴스 생성이 불가능하며 상속(extends)을 위해서면 존재하는 클래스다. 다른 클래스로 파생되는 기반 클래스 역할을 하며, 구현 클래스의 형태를 제한한다.

아래는 추상 메서드 `getName()`를 갖는 추상 클래스를 선언하는 방법이다:

```ts
// 코드 출처: https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members
abstract class Base {
  abstract getName(): string;
 
  printName() {
    console.log("Hello, " + this.getName());
  }
}

class Derived extends Base {
  getName() {
    return "world";
  }
}
 
const d = new Derived();
d.printName();
```

구현 클래스는 반드시 `getName()`을 구체화 해야 한다. 그렇지 않으면 에러가 발생한다:

```ts
class Derived extends Base {} // error TS18052: Non-abstract class 'Derived' does not implement all abstract members of 'Base'
```

### 추상 생성자 시그니처 Abstract Construct Signatures

타입스크립트에선 파라미터에 대한 타입으로 생성자를 선언할 수 있다. 이것은 그 중 추상 클래스의 생성자 타입을 제한하는 방법이다. (별 게 다 있다 🥲)

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

추상 클래스 `Base`와 `Base`를 상속하는 `Derived`가 있을 때, `greet()` 함수에서 `Base`와 `Base`의 서브 클래스 생성자를 파라미터로 받도록 타입을 제한했다. 그리고 `Derived` 생성자 함수를 인수로 넘기는 코드다.

만약 `Base` 생성자 함수를 인수로 하면:

```ts
greet(Base);
// error TS2345: Argument of type 'typeof Base' is not assignable to parameter of type 'new () => Base'.
// Cannot assign an abstract constructor type to a non-abstract constructor type.
```

추상 클래스는 인스턴스를 만들 수 없기 때문에 에러가 발생한다.

🚨 `typeof` 타입 연산자로도 비슷한 구현이 가능하지만 권장되지 않는다.


## 열거형 Enums

[TypeScript Documentation \| Enums](https://www.typescriptlang.org/ko/docs/handbook/enums.html)

**TODO**


## 커스텀 타입 구성하기

### 유니언 Unions

여러 타입 중 하나일 수 있음을 나타내는 방법이다. 허용할 타입들은 파이프`|`로 구분한다.

```ts
type myNumber = number | string;
let n: myNumber = 1;
n = '1'; // OK
n = false; // error TS2322: Type 'boolean' is not assignable to type 'myNumber'.

type myNumber2 = 1 | 2 | 3 | '1' | '2' | '3';
let n2: myNumber2 = 1;
n2 = '3'; // OK
n2 = 4; // error TS2322: Type '4' is not assignable to type 'myNumber2'.

// 배열 내부 요소에 적용
let arr2: myNumber2[] = [1, '2', 3]; // OK
```

```ts
type Person = { name: string, age: number };
type Human = { breathing: boolean };

let waldo: Person | Human;

waldo = {
  name: 'waldo',
  age: 47,
  breathing: true
}; // O

waldo = {
  hello: 'Hello there!'
  // error TS2353: Object literal may only specify known properties, and 'hello' does not exist in type 'Person | Human'.
};
```

### 인터섹션 Intersections

공식 도움말에선 *교집합*으로 번역한다. 여러 타입 중 하나(OR)라도 만족하면 되는 유니언과 다르게 인터섹션은 여러 타입을 모두 만족(AND)해야 한다:

```ts
type Person = { name: string, age: number };
type Human = { breathing: boolean };

let waldo: Person & Human;

waldo = {
  name: 'waldo', 
  age: 47,
  breathing: true,
}; // O

waldo = {
  name: 'waldo', 
  age: 47
  // error TS2322: Type '{ name: string; age: number; }' is not assignable to type 'Person & Human'.
  // Property 'breathing' is missing in type '{ name: string; age: number; }' but required in type 'Human'.
};
```

인터섹션 타입으로 원시 타입은 지정할 수 없다. 숫자이면서 동시에 문자열인 값은 존재할 수 없기 때문:

```ts
type conflicting = number & string;

let foo1: conflicting;
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

type ObjectWithNameArray = Array<{ name: string }>;
let objArr: ObjectWithNameArray = [{ name: '1' }, { name: '2' }];
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
  },
}

const object = backpack.get();
console.log(object); // []

backpack.add('23'); // OK
console.log(object); // ['23']

backpack.add(23); // error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'.
```


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

const point = { x: 12, y: 26 };
printPoint(point);
```

`point`는 `Point` 타입으로 선언된 적이 없지만, 타입 검사기가 알아서 형태를 비교하고 통과시킨다. 따라서 이 코드는 정상이다.

형태를 비교할 때는 부분 집합인지만 통과하면 된다. 

```ts
const point2 = { x: 12, y: 26, z: 89 }; // OK
const rect = { x: 33, y: 3, width: 30, height: 80 }; // OK
```

하지만 전혀 엉뚱한 구조라면:

```ts
const color = { hex: "#187ABF" };
// error TS2345: Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
// Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

반기는 것은 타입 에러다.


## 앰비언트 선언 Ambient Declarations

`declare` 키워드로 변수나 모듈, 함수, 클래스 등의 존재를 타입스크립트에 알리기 위해 사용하는 선언 방식.

```ts
declare var myVariable: string;
declare function myFunction(a: number): void;
declare class MyClass { }
```

앰비언트 선언은 보통 `.d.ts` 파일(타입스크립트 선언 파일)에 작성한다. 이 파일들은 컴파일 중 코드로 변환되지 않으며, 오직 타입 체크와 자동 완성, 문서화를 위해 사용된다.

**TODO** 설명 추가

[TypeScript Documentation \| Creating .d.ts Files from .js files](https://www.typescriptlang.org/docs/handbook/declaration-files/dts-from-js.html)


## 다운레벨링 Downleveling

다운레벨링은 상위 버전의 스크립트를 하위 버전으로 바꿔주는 기능을 의미하는 용어다. 

아래처럼 ES2015에 도입된 기술은 템플릿 리터럴을 사용한 코드는:

```ts
let person = 'John Doe';
`Hello ${person}.`;
```

`tsconfig.json`의 `target` 설정이 ES2015보다 낮은 ES5라면 아래처럼 컴파일된다:

```js
"use strict";
var person = 'John Doe';
"Hello ".concat(person, ".");
```

하지만 다운레벨링이 모든 것을 가능하게 해주는 요술봉은 아니다. 예를 들어 ES5에서 `symbol` 타입은 사용할 수 없기 때문에 컴파일 에러가 발생한다:

```ts
// "target": "es5"
let s: symbol = Symbol('s'); 
// error TS2585: 'Symbol' only refers to a type, but is being used as a value here. Do you need to change your target library? Try changing the 'lib' compiler option to es2015 or later.
```


## 연산자

### Keyof 타입 연산자

[TypeScript Documentation \| Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

**TODO** 

### Typeof 타입 연산자

[TypeScript Documentation \| Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

**TODO**


## 타입스크립트의 JSDoc

[TypeScript Documentation \| JSDoc Reference](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)

**TODO**
