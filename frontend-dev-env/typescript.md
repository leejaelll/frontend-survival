## π κ°μ μ λ¦¬

- JavaScript νμμ΄ μ ννμ§ νμΈνλ λκ΅¬
- κ°λ¨ν [REPL](#REPL)μ μ°κ³  μΆλ€λ©΄ ts-nodeλ₯Ό μ€ννλ©΄ λλ€.

**ts-node**

- μλ°μ€ν¬λ¦½νΈλ₯Ό λΈλΌμ°μ κ° μλ νκ²½μμ μ€νμν€κ³  μΆμ λ nodeλ₯Ό μ€μΉν κ²μ²λΌ νμμ€ν¬λ¦½νΈλ₯Ό nodeμμ μ€νμν¬ μ μλ€.
- νμ§λ§ νμμ€ν¬λ¦½νΈλ‘ μμ±λ .ts νμΌμ λ¨μν nodeλ‘λ§μ μ€νμν¬ μ μλ€. nodeλ νμμ€ν¬λ¦½νΈλ₯Ό ν΄μνμ§ λͺ»νκΈ° λλ¬Έμ΄λ€. ππ»Β μ΄λ νμνκ² `ts-node`
- ./node_modules/.binμλ ts-nodeκ° μλ€. κ·ΈλΌ npm installλ‘ μ€μΉλ₯Ό ν΄μΌνλ? ππ»Β npxλ₯Ό μ¬μ©νλ©΄ λλ€!
  ```jsx
  npx ts-node
  ```

<br>

## νμ μ§μ 

νμμ€ν¬λ¦½νΈμ κ°μ₯ κΈ°λ³Έ ππ» νμμ μ‘μμ£Όλ κ²!

λ§μ μΈμ΄λ€μ΄ `:type` κ°μ λ°©μμ μ¬μ©ν΄μ νμμ μ§μ νλλ° νμμ€ν¬λ¦½νΈλ λΉμ·ν λ°©μμ λ°λΌκ°λ€.

```jsx
let name: string;
name = 'μ΄μ¬λ¦°';
name; // μ΄μ¬λ¦°
```

nameμ μ«μλ₯Ό ν λΉνλ©΄?

```jsx
name = 29; // error TS2322: Type 'number' is not assignable to type 'string'.
```

κ°μ²΄μ κ²½μ°λ μ΄λ»κ² ν΄μΌν κΉ?

```jsx
let human: {
  name: string,
  age: number,
};

human = { name: 'μ΄μ¬λ¦°', age: 29 };

// humanμ λΉ κ°μ²΄λ₯Ό ν λΉνλ©΄
human = {}; //  error TS2739: Type '{}' is missing the following properties from type '{ name: string; age: number; }': name, age
```

- `let human` μ humanμ΄λΌλ μλ³μμλ§ νμμ μ§μ ν΄λμ μν
- κ°μ²΄ μμ μλ νλ‘νΌν°μ νμμ μ§μ νκ³  μΆλ€λ©΄? ππ»Β μ¬μ¬μ©ν΄μ μ¬μ©ν  μ μλλ‘ νμ μμ²΄λ₯Ό μ μν  μ μλ€.

<br>

**νμ(type) μ¬μ©νλ λ°©λ²**

```jsx
// λλ§μ νμ μμ±
type Human = {
  name: string,
  age: number,
};

let boy: Human;

// boyμ λΉ κ°μ²΄λ§ ν λΉνλ©΄ μλ¬ λ©μΈμ§λ₯Ό λ³΄μ¬μ€λ€.
// Human νμμ name, ageλ₯Ό μ°μ§ μμλ€λ μλ¬
boy = {}; // error TS2739: Type '{}' is missing the following properties from type 'Human': name, age

boy = { name: 'κΈΈλ', age: 12 };
```

<br>

**interface μ¬μ©νλ λ°©λ²**

```jsx
interface Person: {
	name: string;
	age: number;
}

let girl: Person;

girl = {} // error TS2739: Type '{}' is missing the following properties from type 'Person': name, age

girl = { name: "κΈΈλ", age: 12};
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

νμμ μ ν΄μ§ κ°μΌλ‘ μ§μ ν  μλ μλ€.

```jsx
let category: 'food';

category = 'food';
```

- μ΄λ° νμμ Unionsμμ μ μ©νκ² μ°μΈλ€.

λ°°μ΄λ‘ νμμ μ§μ νκ³  μΆλ€λ©΄?

```jsx
let numbers: number[];

numbers = [1, 2, 3];
```

λ°°μ΄μ λ€μ΄μ€λ μμμ νμμ΄ [string, number] νμμ΄ λλλ‘ λ§λ€κ³  μΆλ€λ©΄ Tupleμ μ¬μ©νλ€.

```jsx
// anyλ μ΄λ€ νμμ λ£μ΄λ μκ΄μλ€.
let anythings: any[];

anythings = ['hp', 256];

let pair: [string, number];

pair = ['hp', 256];
pair = ['hp', '256']; //error TS2322: Type 'string' is not assignable to type 'number'.
```

<br>

## νμ μΆλ‘  (Types by Inference)

TypeScriptλ JavaScriptμ λ¬Έλ²μ μκ³  μμ ππ»Β λ³μλ₯Ό μμ±νλ©΄μ κ°μ ν λΉνλ©΄ λ³μ νμμ κ·Έ κ°μ νμμΌλ‘ μ§μ νλ€.

TypeScriptκ° νμμ μΆλ‘ ν΄μ μ§μ νλ©΄ λ€λ₯Έ κ°μ λ£μμ λ μ€λ₯λ₯Ό λ°μμν¬κΉ?

```jsx
let age = 10;

// ageμ string νμμ κ°μ ν λΉνλ©΄? μλ¬λ₯Ό λ°μμν¨λ€!
age = 'Lee'; // error TS2322: Type 'string' is not assignable to type 'number'.
```

<br>

## Union Type

unionμ κ²°ν©μ μλ―Ένλ©° μνμμλ ν©μ§ν©μ μλ―Ένλ€.

μ¦, νμμ΄ μ¬λ¬ νμ μ€μ νλμΌ μ μλ€κ³  μλ €μ£Όλ κ²

```jsx
type bool = true | false;

let flag: bool;

flag = true;

flag = false;

flag = 3; // error TS2322: Type 'number' is not assignable to type 'bool'.
```

μ λμΈ νμμ μΈμ  μ¬μ©ν κΉ?

1. νμ©λλΒ `string`Β λλΒ `number`μΒ λ¦¬ν°λ΄μ§ν©μ μ€λͺν  λ

   ```jsx
   type WindowStates = 'open' | 'closed' | 'minimized';
   type LockStates = 'locked' | 'unlocked';
   type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

   // λ§€κ°λ³μλ₯Ό μ νν  λ μμ£Ό μ¬μ©λλ€.
   type Category = 'food' | 'toy' | 'bag';

   function fetchProducts({ category }: { category: Category }) {
     console.log(`Fetch ${category}`);
   }
   ```

   νμμ λ κ° μ€ νλ κ°μ§ μ μλλ‘ λ§λ€μ΄λ³΄μ.

   ```jsx
   let target: number | string;

   target = 12;
   target = 'mike';
   taget = undefined; // error TS2322: Type 'undefined' is not assignable to type 'string | number'.
   ```

   - parameterμ νμμ μ§μ νκ² λλ©΄ λ°λμ μ§μ ν parameterλ₯Ό λ£μ΄μΌ νλ€.

     ```jsx
     function add(x: number, y: number): number {
       return x + y;
     }

     add(1); // error TS2554: Expected 2 arguments, but got 1.
     ```

   - νμ§λ§ argumentsλ₯Ό μ§μ ν parameterλ³΄λ€ μ κ² λ£μ΄λ κ°μ λ¦¬ν΄μμΌμ£Όκ³  μΆλ€λ©΄? ππ»Β μ΄λ΄ λ Optional Parameterλ₯Ό μ¬μ©νλ€!

     ```jsx
     function add(x: number, y?: number | undefined) {
       return x + (y || 0);
     }

     // Optional Parameterλ₯Ό μ¬μ©ν  λ undefinedλ₯Ό μλ΅ν΄λ λλ€.
     function add(x: number, y?: number) {
       return x + (y || 0);
     }
     ```

   - κ°μ₯ μΆμ²νλ κ²μ κΈ°λ³Έ κ°μ λ£μ΄μ£Όλ κ²μ μΆμ²νλ€.
     ```jsx
     function add(x: number, y = 0) {
       return x + y;
     }
     ```

2. λ§€κ°λ³μκ° μ€λΈμ νΈμΌ λλ νμ©ν  μ μλ€.

   ```jsx
   function greeting({ name, age }: { name: string, age?: number }): string {
     return age ? `${name} (${age})` : name;
   }
   ```

   {% hint style="danger" %}
   ts-nodeμμλ μ¬λ¬ μ€λ‘ μΌμ λ ν΄μν  μ μμ΄μ μλ¬κ° λ°μνλ€.
   {% endhint %}

   <br>

## Intersection Type

μ λμΈκ³Ό λλΆμ΄ TypeScriptμ κ΅μ§ν©κΉμ§ κ°μ§κ³  μμ

intersection typeμ & μ°μ°μλ₯Ό μ¬μ©ν΄μ μ μνλ€.

```jsx
type Human = {
  name: string,
  age: number,
};

type Creature = {
  hp: number,
  mp: number,
};

// Humanκ³Ό Creatureμ λ λ€ λ§μ‘±ν΄μΌνλ€.
type Person = Human & Creature;

let person: Person = { name: 'νκΈΈλ', age: 13, hp: 256, mp: 16 };
person = { name: 'νκΈΈλ', age: 13 }; // error TS2322: Type '{ name: string; age: number; }' is not assignable to type 'Person'. Type '{ name: string; age: number; }' is missing the following properties from type 'Creature': hp, mp
```

<br>

## Generics, Utility Types, and Tips

### Generics

νμμ λ³μλ₯Ό μ κ³΅νλ λ°©λ²

### Utilty Types

[Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) ππ»Β Genericsλ₯Ό μ΄μ©ν΄μ λ§λ  νμ

_(Genericμ΄ μ΅μν΄μ§λ©΄ κ³΅λΆνκΈ° μ’μ)_

<br>

## Visual Studio Code μλ μμ± + μ€μκ° μ€λ₯ κ²μ¬

- TypeScriptλ₯Ό μ°λ κ°μ₯ ν° μ΄μ 
- μ€λλ λΌμ΄λΈλ¬λ¦¬μ κ²½μ°μλ d.ts νμΌλ§ λ°λ‘ μ κ³΅λλ€. ν¨ν€μ§ μ΄λ¦μ β@types/~ β νν

---

## β Keyword

### REPL

ππ» REPLμ΄λ?

- Read - Eval - Print Loop
- λνν νλ‘κ·Έλλ° νκ²½μ μλ―Έ
- REPLμ μ¬μ©μκ° μλ ₯ν μ½λλ₯Ό μ½μ΄λ€μ΄κ³ , μ€ννμ¬ κ²°κ³Όλ₯Ό μΆλ ₯νλ©°, μ΄ κ³Όμ μ λ°λ³΅μ μΌλ‘ μννλ€.

<br>

### Interface vs Type

{% hint style="info" %}

### κ°μ²΄λ₯Ό μ μνλ λ κ°μ§ λ°©λ²

ππ» `interfaces` and `type aliases`
{% endhint %}

ππ»ββοΈ Typeκ³Ό Interface λ°©μμ μ°¨μ΄μ μ΄ μμκΉ?

- μΌλ°μ μΈ κ²½μ° λμΌνκ² λμνλ€.

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

- μ ν νμ₯μ μμ΄μ μ°¨μ΄κ° μλ€.
  - Type aliasesλ intersection typesμ ν΅ν΄ μν
  - interfaceλ ν€μλλ₯Ό ν΅ν΄ μν

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

ππ» Union Type

- | κΈ°νΈλ₯Ό μ¬μ©νμ¬ μ¬λ¬ νμ μ€ νλμΌ μ μλ νμμ λ§λλ λ°©λ²
- `string | number`λ string νμ λλ number νμ μ€ νλ

ππ» Intersection Type

- & κΈ°νΈλ₯Ό μ¬μ©νμ¬ μ¬λ¬ νμμ΄ λͺ¨λ λ§μ‘±νλ νμμ λ§λλ λ°©λ²
- `Person & Serializable`μ `Person` νμκ³Ό `Serializable` νμμ λͺ¨λ  μμ±μ΄ ν¬ν¨λμ΄ μλ νμ

<br>

### Optional Parameter

- ν¨μμ λ§€κ°λ³μ μ€ μΌλΆλ₯Ό μ νμ μΌλ‘ μ§μ ν  μ μλ λ°©λ²
- λ§€κ°λ³μ μ΄λ¦ λ€μ ? κΈ°νΈλ₯Ό μ¬μ©νμ¬ μ§μ 

```jsx
function myFunction(a: string, b?: number) {
  console.log(a, b);
}
```

- myFunction ν¨μλ a λ§€κ°λ³μλ λ°λμ μ λ¬λμ΄μΌ νμ§λ§, b λ§€κ°λ³μλ μ νμ μΌλ‘ μ λ¬ν  μ μλ€.
- ν¨μ λ΄λΆμμλ b λ§€κ°λ³μκ° μ λ¬λμ§ μμμ κ²½μ°, undefined κ°μ κ°λλ€.
- Optional Parameter λ μ’μ λ°©λ²μ? ππ» λ§€κ°λ³μμ κΈ°λ³Έ κ°μ λ£μ΄μ£Όλ κ²!

---

## π Supplement

### λ¦¬ν°λ΄ νμ(Literal Type)

π¬ μλ°μ€ν¬λ¦½νΈμμ λ¦¬ν°λ΄μ΄λΌλ κ°λμ λ°°μ μλλ°, μλ°μ€ν¬λ¦½νΈμμ λ§νλ λ¦¬ν°λ΄κ³Ό λμΌνκ±΄κ°?

- μλ°μ€ν¬λ¦½νΈμμμ λ¦¬ν°λ΄μ μλ―Έ  
  : λ¬Έμ λλ μ½μλ κΈ°νΈλ₯Ό μ¬μ©ν΄ κ°μ μμ±νλ νκΈ°λ²

- νμμ€ν¬λ¦½νΈμμμ λ¦¬ν°λ΄ νμμ΄ μλ―Ένλκ±΄?  
  : νΉμ  κ°μ΄ κ°μ§ μ μλ μ νν κ°μ μ§ν©μ μ μνλ νμ

- λ¦¬ν°λ΄ νμμ **κ°μ μ νμ μ μΈνλ©΄μ λμμ κ°μ λ²μ**λ₯Ό κ°κ² νλ€.

- λ¦¬ν°λ΄ νμ μ μ λ°©λ²

  ```jsx
  type MyString = 'Hello' | 'World';
  type MyNumber = 1 | 2 | 3;
  ```

<br>

## Generics

- μ λ€λ¦­μ **νμμ λ³μλ₯Ό μ κ³΅**νλ λ°©λ²

> _λ°°μ΄μ΄ μΌλ°μ μΈ μμμ΄λ©°, μ λ€λ¦­μ΄ μλ λ°°μ΄μ μ΄λ€ κ²μ΄λ  ν¬ν¨ν  μ μμ΅λλ€. μ λ€λ¦­μ΄ μλ λ°°μ΄μ λ°°μ΄ μμ κ°μ μ€λͺν  μ μμ΅λλ€._

π¬ νμμ€ν¬λ¦½νΈ κ³΅μλ¬Έμμμ λ§ν΄μ£Όλ λ΄μ©μ΄ μ ννκ² μ΄ν΄κ° κ°μ§ μλλ€. μ λ€λ¦­μ΄ μ ννκ² λ¬΄μμ΄κ³  μ΄λ»κ² μ¬μ©ν΄μΌ νλκ±ΈκΉ?

- Javaλ C# κ°μ μ μ  νμ μΈμ΄μ κ²½μ°, ν¨μ λλ ν΄λμ€λ₯Ό μ μνλ μμ μ λ§€κ°λ³μλ λ°νκ°μ νμμ μ μΈνμ¬μΌ νλ€.
- νμμ€ν¬λ¦½νΈ λν μ μ  νμ μΈμ΄μ΄κΈ° λλ¬Έμ ν¨μ λλ ν΄λμ€λ₯Ό μ μνλ μμ μ λ§€κ°λ³μλ λ°νκ°μ νμμ μ μΈν΄μΌ νλ€.

**κ·Έλ°λ° ν¨μ λλ ν΄λμ€λ₯Ό μ μνλ μμ μ λ§€κ°λ³μλ λ°νκ°μ νμμ μ μΈνκΈ° μ΄λ €μ΄ κ²½μ°κ° μλ€.**

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
queue.push('1'); // μλνμ§ μμ μ€μ!

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // Runtime error
```

- Queue ν΄λμ€μ data νλ‘νΌν°λ any[] νμ
- μ μμ μ κ²½μ° data νλ‘νΌν°λ number νμλ§μ ν¬ν¨νλ λ°°μ΄μ΄λΌλ κΈ°λ νμ κ° μμμ λν΄ Number.prototype.toFixedλ₯Ό μ¬μ©ν¨
- λ°λΌμ number νμμ΄ μλ μμμ κ²½μ° λ°νμ μλ¬κ° λ°μ

μμ κ°μ λ¬Έμ λ₯Ό ν΄κ²°νκΈ° μν΄ Queue ν΄λμ€λ₯Ό μμνμ¬ number νμ μ μ© NumberQueue ν΄λμ€λ₯Ό μ μ

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

// Queue ν΄λμ€λ₯Ό μμνμ¬ number νμ μ μ© NumberQueue ν΄λμ€λ₯Ό μ μ
class NumberQueue extends Queue {
  // number νμμ μμλ§μ pushνλ€.
  push(item: number) {
    super.push(item);
  }

  pop(): number {
    return super.pop();
  }
}

const queue = new NumberQueue();

queue.push(0);
// μλνμ§ μμ μ€μλ₯Ό μ¬μ  κ²μΆ κ°λ₯
// error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
// queue.push('1');
queue.push(+'1'); // μ€μλ₯Ό μ¬μ  μΈμ§νκ³  μμ ν  μ μλ€

console.log(queue.pop().toFixed()); // 0
console.log(queue.pop().toFixed()); // 1
```

- νμ§λ§ λ€μν νμμ μ§μν΄μΌ νλ€λ©΄ νμ λ³λ‘ ν΄λμ€λ₯Ό μμλ°μ μΆκ°ν΄μΌ νλ―λ‘ μ΄ λν μ’μ λ°©λ²μ μλλ€.

μ΄λ μ λ€λ¦­μ μ¬μ©νλ©΄ μ΄λ¬ν λ¬Έμ λ₯Ό ν΄κ²°ν  μ μλ€.

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

// number μ μ© Queue
const numberQueue = new Queue<number>();

numberQueue.push(0);
// numberQueue.push('1'); // μλνμ§ μμ μ€μλ₯Ό μ¬μ  κ²μΆ κ°λ₯
numberQueue.push(+'1');   // μ€μλ₯Ό μ¬μ  μΈμ§νκ³  μμ ν  μ μλ€

// ?. => optional chaining
// https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining
console.log(numberQueue.pop()?.toFixed()); // 0
console.log(numberQueue.pop()?.toFixed()); // 1
console.log(numberQueue.pop()?.toFixed()); // undefined

// string μ μ© Queue
const stringQueue = new Queue<string>();

stringQueue.push('Hello');
stringQueue.push('World');

console.log(stringQueue.pop()?.toUpperCase()); // HELLO
console.log(stringQueue.pop()?.toUpperCase()); // WORLD
console.log(stringQueue.pop()?.toUpperCase()); // undefined

// μ»€μ€ν κ°μ²΄ μ μ© Queue
const myQueue = new Queue<{name: string, age: number}>();
myQueue.push({name: 'Lee', age: 10});
myQueue.push({name: 'Kim', age: 20});

console.log(myQueue.pop()); // { name: 'Lee', age: 10 }
console.log(myQueue.pop()); // { name: 'Kim', age: 20 }
console.log(myQueue.pop()); // undefined
```

{% hint style="success" %}
μ λ€λ¦­μ μ μΈ μμ μ΄ μλλΌ μμ± μμ μ νμμ λͺμνμ¬ νλμ νμλ§μ΄ μλ λ€μν νμμ μ¬μ©ν  μ μλλ‘ νλ κΈ°λ²μ΄λ€. νλ²μ μ μΈμΌλ‘ λ€μν νμμ μ¬μ¬μ©μ΄ κ°λ₯νλ€λ μ₯μ μ΄ μλ€.
{% endhint %}

π¬ μ¬κΈ°μ Tλ λ¬΄μμ μλ―Έν κΉ?

- μ λ€λ¦­μ μ μΈν  λ κ΄μ©μ μΌλ‘ μ¬μ©λλ μλ³μ
- νμ νλΌλ―Έν°(Type parameter) λΌκ³ λ νλ€.

<br>

+) [λ μ’μ νμμ€ν¬λ¦½νΈ νλ‘κ·Έλλ¨Έλ‘ λ§λλ 11κ°μ§ ν](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)
