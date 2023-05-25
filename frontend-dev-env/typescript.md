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

   {% hint style="danger" %}
   ts-node에서는 여러 줄로 썼을 때 해석할 수 없어서 에러가 발생한다.
   {% endhint %}

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

🙋🏻 REPL이란?

- Read - Eval - Print Loop
- 대화형 프로그래밍 환경을 의미
- REPL은 사용자가 입력한 코드를 읽어들이고, 실행하여 결과를 출력하며, 이 과정을 반복적으로 수행한다.

<br>

### Interface vs Type

{% hint style="info" %}

### 객체를 정의하는 두 가지 방법

👉🏻 `interfaces` and `type aliases`
{% endhint %}

🙋🏻‍♀️ Type과 Interface 방식의 차이점이 있을까?

- 일반적인 경우 동일하게 동작한다.

```jsx
type BirdType = {
  wings: 2,
};

interface BirdInterface {
  wings: 2;
}

const bird1: BirdType = { wings: 2 };
const bird2: BirdInterface = { wings: 2 };

const bird3: BirdInterface = bird1;
```

- 유형 확장에 있어서 차이가 있다.
  - Type aliases는 intersection types을 통해 수행
  - interface는 키워드를 통해 수행

```jsx
type Owl = { nocturnal: true } & BirdType;
type Robin = { nocturnal: false } & BirdInterface;
```

```jsx
interface Peacock extends BirdType {
  colourful: true;
  flies: false;
}
interface Chicken extends BirdInterface {
  colourful: false;
  flies: false;
}
```

<br>

### Union Type Vs. Intersection Type

🙋🏻 Union Type

- | 기호를 사용하여 여러 타입 중 하나일 수 있는 타입을 만드는 방법
- `string | number`는 string 타입 또는 number 타입 중 하나

🙋🏻 Intersection Type

- & 기호를 사용하여 여러 타입이 모두 만족하는 타입을 만드는 방법
- `Person & Serializable`은 `Person` 타입과 `Serializable` 타입의 모든 속성이 포함되어 있는 타입

<br>

### Optional Parameter

- 함수의 매개변수 중 일부를 선택적으로 지정할 수 있는 방법
- 매개변수 이름 뒤에 ? 기호를 사용하여 지정

```jsx
function myFunction(a: string, b?: number) {
  console.log(a, b);
}
```

- myFunction 함수는 a 매개변수는 반드시 전달되어야 하지만, b 매개변수는 선택적으로 전달할 수 있다.
- 함수 내부에서는 b 매개변수가 전달되지 않았을 경우, undefined 값을 갖는다.
- Optional Parameter 더 좋은 방법은? 👉🏻 매개변수에 기본 값을 넣어주는 것!

---

## 🐋 Supplement

### 리터럴 타입(Literal Type)

💬 자바스크립트에서 리터럴이라는 개념을 배웠었는데, 자바스크립트에서 말하는 리터럴과 동일한건가?

- 자바스크립트에서의 리터럴의 의미  
  : 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법

- 타입스크립트에서의 리터럴 타입이 의미하는건?  
  : 특정 값이 가질 수 있는 정확한 값의 집합을 정의하는 타입

- 리터럴 타입은 **값의 유형을 선언하면서 동시의 값의 범위**를 갖게 한다.

- 리터럴 타입 정의 방법

  ```jsx
  type MyString = 'Hello' | 'World';
  type MyNumber = 1 | 2 | 3;
  ```

<br>

## Generics

- 제네릭은 **타입에 변수를 제공**하는 방법

> _배열이 일반적인 예시이며, 제네릭이 없는 배열은 어떤 것이든 포함할 수 있습니다. 제네릭이 있는 배열은 배열 안의 값을 설명할 수 있습니다._

💬 타입스크립트 공식문서에서 말해주는 내용이 정확하게 이해가 가지 않는다. 제네릭이 정확하게 무엇이고 어떻게 사용해야 하는걸까?

- Java나 C# 같은 정적 타입 언어의 경우, 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언하여야 한다.
- 타입스크립트 또한 정적 타입 언어이기 때문에 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언해야 한다.

**그런데 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언하기 어려운 경우가 있다.**

```jsx
class Queue {
  protected data: any[] = [];

  push(item: any) {
    this.data.push(item);
  }

  pop() {
    return this.data.shift();
  }
}

const queue = new Queue();

queue.push(0);
queue.push('1'); // 의도하지 않은 실수!

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // Runtime error
```

- Queue 클래스의 data 프로퍼티는 any[] 타입
- 위 예제의 경우 data 프로퍼티는 number 타입만을 포함하는 배열이라는 기대 하에 각 요소에 대해 Number.prototype.toFixed를 사용함
- 따라서 number 타입이 아닌 요소의 경우 런타임 에러가 발생

위와 같은 문제를 해결하기 위해 Queue 클래스를 상속하여 number 타입 전용 NumberQueue 클래스를 정의

```jsx
class Queue {
  protected data: any[] = [];

  push(item: any) {
    this.data.push(item);
  }

  pop() {
    return this.data.shift();
  }
}

// Queue 클래스를 상속하여 number 타입 전용 NumberQueue 클래스를 정의
class NumberQueue extends Queue {
  // number 타입의 요소만을 push한다.
  push(item: number) {
    super.push(item);
  }

  pop(): number {
    return super.pop();
  }
}

const queue = new NumberQueue();

queue.push(0);
// 의도하지 않은 실수를 사전 검출 가능
// error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
// queue.push('1');
queue.push(+'1'); // 실수를 사전 인지하고 수정할 수 있다

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // 1
```

- 하지만 다양한 타입을 지원해야 한다면 타입 별로 클래스를 상속받아 추가해야 하므로 이 또한 좋은 방법은 아니다.

이때 제네릭을 사용하면 이러한 문제를 해결할 수 있다.

```jsx
class Queue<T> {
  protected data: Array<T> = [];
  push(item: T) {
    this.data.push(item);
  }
  pop(): T | undefined {
    return this.data.shift();
  }
}

// number 전용 Queue
const numberQueue = new Queue<number>();

numberQueue.push(0);
// numberQueue.push('1'); // 의도하지 않은 실수를 사전 검출 가능
numberQueue.push(+'1');   // 실수를 사전 인지하고 수정할 수 있다

// ?. => optional chaining
// https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining
console.log(numberQueue.pop()?.toFixed()); // 0
console.log(numberQueue.pop()?.toFixed()); // 1
console.log(numberQueue.pop()?.toFixed()); // undefined

// string 전용 Queue
const stringQueue = new Queue<string>();

stringQueue.push('Hello');
stringQueue.push('World');

console.log(stringQueue.pop()?.toUpperCase()); // HELLO
console.log(stringQueue.pop()?.toUpperCase()); // WORLD
console.log(stringQueue.pop()?.toUpperCase()); // undefined

// 커스텀 객체 전용 Queue
const myQueue = new Queue<{name: string, age: number}>();
myQueue.push({name: 'Lee', age: 10});
myQueue.push({name: 'Kim', age: 20});

console.log(myQueue.pop()); // { name: 'Lee', age: 10 }
console.log(myQueue.pop()); // { name: 'Kim', age: 20 }
console.log(myQueue.pop()); // undefined
```

{% hint style="success" %}
제네릭은 선언 시점이 아니라 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법이다. 한번의 선언으로 다양한 타입에 재사용이 가능하다는 장점이 있다.
{% endhint %}

💬 여기서 T는 무엇을 의미할까?

- 제네릭을 선언할 때 관용적으로 사용되는 식별자
- 타입 파라미터(Type parameter) 라고도 한다.

<br>

+) [더 좋은 타입스크립트 프로그래머로 만드는 11가지 팁](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)
