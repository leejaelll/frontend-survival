## 📚 강의 정리

- JavaScript 타입이 정확한지 확인하는 도구
- 간단히 [REPL](#REPL)을 쓰고 싶다면 ts-node를 실행하면 된다.

**ts-node**

- 자바스크립트를 브라우저가 아닌 환경에서 실행시키고 싶을 때 node를 설치한 것처럼 타입스크립트를 node에서 실행시킬 수 있다.
- 하지만 타입스크립트로 작성된 .ts 파일은 단순히 node로만은 실행시킬 수 없다. node는 타입스크립트를 해석하지 못하기 때문이다. 👉🏻 이때 필요한게 `ts-node`
- ./node_modules/.bin에는 ts-node가 없다. 그럼 npm install로 설치를 해야하나? 👉🏻 npx를 사용하면 된다!
  ```jsx
  npx ts-node
  ```

<br>

## 타입 지정

타입스크립트의 가장 기본 👉🏻 타입을 잡아주는 것!

많은 언어들이 `:type` 같은 방식을 사용해서 타입을 지정하는데 타입스크립트도 비슷한 방식을 따라간다.

```jsx
let name: string;
name = '이재린';
name; // 이재린
```

name에 숫자를 할당하면?

```jsx
name = 29; // error TS2322: Type 'number' is not assignable to type 'string'.
```

객체의 경우는 어떻게 해야할까?

```jsx
let human: {
  name: string,
  age: number,
};

human = { name: '이재린', age: 29 };

// human에 빈 객체를 할당하면
human = {}; //  error TS2739: Type '{}' is missing the following properties from type '{ name: string; age: number; }': name, age
```

- `let human` 은 human이라는 식별자에만 타입을 지정해놓은 상태
- 객체 안에 있는 프로퍼티의 타입을 지정하고 싶다면? 👉🏻 재사용해서 사용할 수 있도록 타입 자체를 정의할 수 있다.

<br>

**타입(type) 사용하는 방법**

```jsx
// 나만의 타입 생성
type Human = {
  name: string,
  age: number,
};

let boy: Human;

// boy에 빈 객체만 할당하면 에러 메세지를 보여준다.
// Human 타입에 name, age를 쓰지 않았다는 에러
boy = {}; // error TS2739: Type '{}' is missing the following properties from type 'Human': name, age

boy = { name: '길동', age: 12 };
```

<br>

**interface 사용하는 방법**

```jsx
interface Person: {
	name: string;
	age: number;
}

let girl: Person;

girl = {} // error TS2739: Type '{}' is missing the following properties from type 'Person': name, age

girl = { name: "길동", age: 12};
```

```jsx
function add(x: number, y: number): number {
  return x + y;
}

add(1, 2);

add('Hello', 'World'); // TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.

function sub(x: number, y: number): string {
  return x - y; // TS2322: Type 'number' is not assignable to type 'string'.
}
```

<br>

타입을 정해진 값으로 지정할 수도 있다.

```jsx
let category: 'food';

category = 'food';
```

- 이런 타입은 Unions에서 유용하게 쓰인다.

배열로 타입을 지정하고 싶다면?

```jsx
let numbers: number[];

numbers = [1, 2, 3];
```

배열에 들어오는 요소의 타입이 [string, number] 형식이 되도록 만들고 싶다면 Tuple을 사용한다.

```jsx
// any는 어떤 타입을 넣어도 상관없다.
let anythings: any[];

anythings = ['hp', 256];

let pair: [string, number];

pair = ['hp', 256];
pair = ['hp', '256']; //error TS2322: Type 'string' is not assignable to type 'number'.
```

<br>

## 타입 추론 (Types by Inference)

TypeScript는 JavaScript의 문법을 알고 있음 👉🏻 변수를 생성하면서 값을 할당하면 변수 타입을 그 값의 타입으로 지정한다.

TypeScript가 타입을 추론해서 지정하면 다른 값을 넣었을 때 오류를 발생시킬까?

```jsx
let age = 10;

// age에 string 타입의 값을 할당하면? 에러를 발생시킨다!
age = 'Lee'; // error TS2322: Type 'string' is not assignable to type 'number'.
```

<br>

## Union Type

union은 결합을 의미하며 수학에서는 합집합을 의미한다.

즉, 타입이 여러 타입 중에 하나일 수 있다고 알려주는 것

```jsx
type bool = true | false;

let flag: bool;

flag = true;

flag = false;

flag = 3; // error TS2322: Type 'number' is not assignable to type 'bool'.
```

유니언 타입은 언제 사용할까?

1. 허용되는 `string` 또는 `number`의 리터럴집합을 설명할 때

   ```jsx
   type WindowStates = 'open' | 'closed' | 'minimized';
   type LockStates = 'locked' | 'unlocked';
   type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

   // 매개변수를 제한할 때 자주 사용된다.
   type Category = 'food' | 'toy' | 'bag';

   function fetchProducts({ category }: { category: Category }) {
     console.log(`Fetch ${category}`);
   }
   ```

   타입을 두 개 중 하나 가질 수 있도록 만들어보자.

   ```jsx
   let target: number | string;

   target = 12;
   target = 'mike';
   taget = undefined; // error TS2322: Type 'undefined' is not assignable to type 'string | number'.
   ```

   - parameter에 타입을 지정하게 되면 반드시 지정한 parameter를 넣어야 한다.

     ```jsx
     function add(x: number, y: number): number {
       return x + y;
     }

     add(1); // error TS2554: Expected 2 arguments, but got 1.
     ```

   - 하지만 arguments를 지정한 parameter보다 적게 넣어도 값을 리턴시켜주고 싶다면? 👉🏻 이럴 땐 Optional Parameter를 사용한다!

     ```jsx
     function add(x: number, y?: number | undefined) {
       return x + (y || 0);
     }

     // Optional Parameter를 사용할 땐 undefined를 생략해도 된다.
     function add(x: number, y?: number) {
       return x + (y || 0);
     }
     ```

   - 가장 추천하는 것은 기본 값을 넣어주는 것을 추천한다.
     ```jsx
     function add(x: number, y = 0) {
       return x + y;
     }
     ```

2. 매개변수가 오브젝트일 때도 활용할 수 있다.

   ```jsx
   function greeting({ name, age }: { name: string, age?: number }): string {
     return age ? `${name} (${age})` : name;
   }
   ```

   _⚠️ ts-node에서는 여러 줄로 썼을 때 해석할 수 없어서 에러가 발생한다._

<br>

## Intersection Type

유니언과 더불어 TypeScript은 교집합까지 가지고 있음

intersection type은 & 연산자를 사용해서 정의한다.

```jsx
type Human = {
  name: string,
  age: number,
};

type Creature = {
  hp: number,
  mp: number,
};

// Human과 Creature을 둘 다 만족해야한다.
type Person = Human & Creature;

let person: Person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
person = { name: '홍길동', age: 13 }; // error TS2322: Type '{ name: string; age: number; }' is not assignable to type 'Person'. Type '{ name: string; age: number; }' is missing the following properties from type 'Creature': hp, mp
```

<br>

## Generics, Utility Types, and Tips

### Generics

타입에 변수를 제공하는 방법

### Utilty Types

[Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) 👉🏻 Generics를 이용해서 만든 타입

_(Generic이 익숙해지면 공부하기 좋음)_

<br>

## Visual Studio Code 자동 완성 + 실시간 오류 검사

- TypeScript를 쓰는 가장 큰 이유
- 오래된 라이브러리의 경우에는 d.ts 파일만 따로 제공된다. 패키지 이름은 ‘@types/~ ‘ 형태

---

## ✅ Keyword

### REPL

### Interface vs Type

### Union Type vs Intersection Type

### Optional Parameter

---

## 🐋 Supplement

- 리터럴타입
- Generics
- [더 좋은 타입스크립트 프로그래머로 만드는 11가지 팁](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)
