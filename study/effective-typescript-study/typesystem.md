# 타입스크립트의 타입시스템

> _**타입스크립트의 가장 중요한 역할은 타입시스템에 있다.**_

| 타입시스템이란 무엇인가?                  |
| ----------------------------------------- |
| 타입 시스템을 어떻게 사용해야할까?        |
| 무엇을 결정해야하는가?                    |
| 가급적 사용하지 말아야할 기능은 무엇일까? |

### 타입이 값들의 집합이라고 생각하기

{% hint style="info" %}
**never 타입은 언제 사용하는 걸까?**

- 아무 값도 포함하지 않는 **공집합**
- never 타입으로 선언된 변수에는 아무런 값도 할당할 수 없다.

```typescript
function hello(): never {
  throw new Error('Error');
}
```

_**never는 따로 return하지 않고, 오류를 발생시키는 함수에 타입을 지정할 때 사용한다.**_

**unknown**

- 변수의 타입을 모를때 unknown으로 지정할 수 있다.

```typescript
let a: unknown;
if (typeof a === number) {
  let b = a + 1;
}
```

<mark style="color:red;">**any와 unknown를 분리해놓은 이유가 무엇일까?**</mark>

이 둘의 차이점은 무엇일까? any와 unknwon 둘 다 모든 타입을 허용하는 것은 동일하다. 하지만 any를 사용하게 되면 타입검사를 느슨하게 하므로 개발과정에서 오류를 발생시키지 않는다. 반면 unknown의 경우에는 프로퍼티를 사용하거나 연산을 하게 될 경우, 타입체크를 하게된다. 그러므로 문제가 되는 코드를 미리 예방할 수 있다.
{% endhint %}

#### union, 합집합

```typescript
type A = 'A';
type B = 'B';
type Twlve = 12;

type AB = 'A' | 'B';
type AB12 = 'A' | 'B' | 12;
```

#### intersection, 교집합

> _타입 연산자는 인터페이스의 속성이 아닌, **값의 집합**에 적용된다._

```typescript
interface Person {
  name: string;
}
interface Lifespan {
  birth: Date;
  death?: Date;
}
type PersonSpan = Person & Lifespan;

const ps: PersonSpan = {
  name: 'Alan',
  birth: new Date('1912/12/01'),
  death: new Date('2023/08/11'),
}; // ok
```

- 세 개의 속성보다 더 많은 값을 가지더라도 PersonSpan 타입에 속한다.
- 인터섹션 타입의 값은 각 타입 내의 속성을 모두 포함하는 것이 일반적인 규칙

{% hint style="info" %}
**typeof / keyof의 차이점**

**`typeof`**

: 데이터를 데이터 타입으로 변환해주는 연산자

```typescript
let str1 = 'hello';
let str2: typeof str = 'hi';
// === let str2: string ="hi"
```

**`keyof`**

: 객체 형태의 타입을, 따로 속성들만 뽑아 모아 **유니온** 타입으로 만들어주는 연산자

```typescript
type Type = {
  name: string;
  age: number;
  married: boolean;
};

type Union = keyof Type;
// type Union = name | age | married
```

{% endhint %}

유니온 타입에서는 다르게 동작한다.

```typescript
type K = keyof (Person | Lifespan); // never
```

유니온 타입에 속하는 값은 어떠한 키도 없기 때문에 유니온에 대한 keyof는 공집합(never)이어야만 한다.

```typescript
keyof (A&B) = (keyof A) | (keyof B)
keyof (A|B) = (keyof A) & (keyof B)
```

---

### 타입 공간과 값 공간의 심벌 구분하기

심벌(symbol)은 타입 공간이나 값 공간 중의 한 곳에 존재한다. (이 문장만으로는 정확하게 이해가 가지 않으니, 예시를 살펴보자.)

```typescript
interface Cylinder {
  radius: number;
  height: number;
}

const Cylinder = (radius: number, height: number) => ({ radius, height });
```

- interface Cylinder에서 Cylinder는 타입
- const Cylinder에서 Cylinder와 이름은 같지만 값으로 사용하고 있으며, 아무런 관련이 없다.
- 이런 점이 가끔 오류를 야기함

```typescript
function CalculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    return shape.radius;
  }
}
```

<figure><img src="../../.gitbook/assets/스크린샷 2.png" alt=""><figcaption></figcaption></figure>

- instanceof는 런타임 연산자로 값에 대해서 연산을 한다.
- 그렇기 때문에 타입이 아닌 함수를 참조한다.

> _**class와 enum은 상황에 따라 타입과 값 두 가지 모두 가능한 예약어**_

```typescript
class Cylinder {
  radius: number;
  height: number;
}

function calculateVolume(shape: unknown) {
  if (shape instanceof Cylinder) {
    shape; // ok, type은 Cylinder
    shape.radius; // ok, type은 number
  }
}
```

- 클래스가 타입으로 쓰일 때는 속성과 메서드가 사용된다.
- 반면에 값으로 쓰일 때는 생성자가 사용된다.

#### 연산자 typeof도 다른 기능을 한다.

- 타입으로 쓰일 때는 타입을 반환한다.
- 값으로 쓰일 때는 런타임의 typeof 연산자가 된다.

```typescript
function email(p: Person, subject: string, body: string): Response {}

type T1 = typeof p; // type은 Person
type T2 = typeof email; // type은 (p: Person, subject: string, body: string) => Response

const v1 = typeof p; // 값은 object
const v2 = typeof email; // 값은 function
```

#### 두 공간 사이에서 다른 의미를 가지는 코드패턴들

|         | 값                       | 타입                                                                                           |
| ------- | ------------------------ | ---------------------------------------------------------------------------------------------- |
| this    | 자바스크립트 키워드 this | 다형성(polymorphic) this라고 불리는 this의 타입스크립트 타입                                   |
| &와 \|  | 비트 연산                | 인터섹션과 유니온                                                                              |
| const   | 변수 선언                | 리터럴 또는 리터럴 표현식의 추론된 타입을 바꿈                                                 |
| extends | `class A extends B`      | <p><code>interface A extends B </code> 또는<br><code>Generic&#x3C;T extends number></code></p> |
| in      | for(key in object)       | 매핑된 타입                                                                                    |

{% hint style="danger" %}
**다형성(Polymorphism)이 무엇을 의미하는걸까?**

<code>Polymorphism</code>
: Poly + morphos의 합성어로 '여러가지 다른 형태들'을 의미한다.

_즉, 여러 타입을 받아들임으로써 여러 형태를 가지는 것을 의미_

{% endhint %}

_<mark style="color:red;">**타입스크립트가 정상적으로 동작하지 않는다면 타입 공간과 값 공간을 혼동해 잘못 작성했을 가능성이 크다.**</mark>_

이 코드의 문제점은 무엇일까?

```typescript
export default function eamil({
  person: Person,
  subject: string,
  body: string,
}) {}
```

<figure><img src="../../.gitbook/assets/스크린샷 3.png" alt=""><figcaption></figcaption></figure>

- 값의 관점에서 Person과 string이 해석되었기 때문
- Person이라는 변수명과 string이라는 이름을 가지는 두 개의 변수를 생성하려한 것

문제를 해결하려면 어떻게 해야할까? **타입과 값을 구분**해야한다.

```typescript
function eamil({
  person,
  subject,
  body,
}: {
  person: Person;
  subject: string;
  body: string;
}) {}
```

---

### 타입 단언보다는 타입 선언을 사용하기

{% hint style="success" %}
**변수에 값을 할당하고 타입을 부여하는 방법**

1. 타입 단언
2. 타입 선언
   {% endhint %}

왜 타입 단언보다 타입 선언을 사용하는게 나을까?

```typescript
const alice: Person = {}; // Property 'name' is missing in type '{}' but required in type 'Person'.
const bob = {} as Person;
```

- 첫 번째 줄은 타입 선언, **할당되는 값이 해당 인터페이스를 만족하는지 검사**
- 두 번째 줄은 타입 단언, 타입 단언은 **강제로 타입을 지정했으니 타입 체커에게 오류를 무시하라고 하는 것**

타입 선언과 단언의 차이는 속성을 추가할 때도 마찬가지다.

```typescript
const alice: Person = {
  name: 'Alice',
  occupation: 'TypeScript developer',
  // Type '{ name: string; occupation: string; }' is not assignable to type 'Person'.
  // Object literal may only specify known properties, and 'occupation' does not exist in type 'Person'.
};

const bob = {
  name: 'Bob',
  occupation: 'Javascript developer',
} as Person;
```

- 오류 메세지: 객체 리터럴은 알려진 속성만 지정할 수 있으며, 'Person' 형식에 'occupation'이 없습니다.

_<mark style="color:red;">**화살표 함수의 타입 선언은 추론된 타입이 모호할 때가 있다.**</mark>_
예를 들어, Person 인터페이스를 사용하고 싶다고 가정해보자.

```typescript
const people = ['alice', 'bob', 'jan'].map((name) => ({ name })); // Person을 원했지만 결과는 {name: string}[]...
```

people의 타입은 다음과 같다.

<figure><img src="../../.gitbook/assets/230823-1.png" alt=""><figcaption></figcaption></figure>

people의 타입이 `Person[]`과 같이 나오려면 어떻게 작성해야할까?

```typescript
const people = ['alice', 'bob', 'jan'].map((name): Person => ({ name }));
```

- `(name):Person`은 name의 타입이 없고 리턴 타입이 Person이라고 명시하는 것과 같다.
- 만약 `(name: Person)`이라고 작성했다면 name의 타입이 Person임을 명시하고 반환 타입이 없다는 것과 같다. (소괄호가 매우 중요함!)

{% hint="danger"%}

### 타입 단언이 꼭 필요한 경우

타입 단언은 타입 체커가 추론한 타입보다 작성한 사람이 판단하는 타입이 더 정확할 때 의미가 있다.

```typescript
document.querySelector('#myButton').addEventListener('click', (e) => {
  e.currentTarget; // type은 EventTarget
  const button = e.currentTarget as HTMLButtonElement;
  button; // 타입은 HTMLButtonElement
});
```

- 타입스크립트는 DOM에 접근할 수 없기 때문에 #myButton이 버튼 엘리먼트인지 알지 못한다.
- 이벤트의 currentTarget이 같은 버튼이어야 하는 것인지도 알지 못한다.
- 이럴 때에는 타입 단언문을 쓰는 것이 타당하다.

{% endhint %}

**boolean 부정문을사용해서 null이 아님을 단언하기**

```typescript
const elNull = docuemnt.getElementById('foo'); // HTMLElement | null
const el = document.getElementById('foo')!; // HTMLElement
```

- !를 일반적인 단언문처럼 생각하자.
- 단언문은 컴파일 과정 중에 제거되므로 타입 체커는 알지 못하지만 그 값이 null이 아니라고 확신할 수 있을 때 사용해야한다.
- 만약 null인 경우가 존재한다면 조건문을 사용해야 한다.

---

### 객체 래퍼 타입 피하기

우리가 string에 메서드를 사용할 수 있는 이유는 무엇일까? 바로 래퍼객체 때문이다.

자바스크립트는 기본형을 String 객체로 래핑하고, 메서드를 호출하고, 마지막에 래핑한 객체를 버린다.

**타입을 지정할 때 래퍼 객체 타입을 사용하는 것은 지양하자.**
래퍼 객체를 직접 사용하거나 인스턴스 생성하는 것은 피해야한다. 기본형 타입을 객체 래퍼에 할당하는 구문은 오해하기 쉽고, 굳이 그렇게 할 필요도 없기 때문이다.

String 대신 string, Number 대신 number, Boolean 대신 boolean을 사용해야한다.

---

### 타입과 인터페이스 차이점 알기

**타입과 인터페이스의 유사한점**

```typescript
type TState = {
  name: string;
  capital: string;
};

interface IState {
  name: string;
  capital: string;
}
```

- 대부분의 경우에는 타입과 인터페이스 모두 사용해도 된다.
- 추가 속성과 함께 할당한다면 동일한 오류가 발생한다.

  ```typescript
  const wyoming: TState = {
    name: 'Wyoming',
    capital: 'Cheyenne',
    population: 500_000,
    // Type '{ name: string; capital: string; population: number; }' is not assignable to type 'TState'
  };
  ```

- 인덱스 시그니처는 인터페이스와 타입 모두 사용할 수 있다.

  ```typescript
  type TDict = { [key: string]: string };
  interface IDict {
    [key: string]: string;
  }
  ```

- 함수 타입도 인터페이스와 타입 모두 사용가능하다.

  ```typescript
  type TFn = (x: number) => string;
  interface IFn {
    (x: number): string;
  }
  ```

- 타입별칭과 인터페이스는 모두 제너릭이 가능하다.

  ```typescript
  type TPair<T> = {
    first: T;
    second: T;
  };

  interface IPair<T> {
    first: T;
    second: T;
  }
  ```

{% hint="info"%}

### 인덱스 시그니처 (Index Signature)

`{ [Key: T]: U }` 형식으로 객체가 여러 Key를 가질 수 있으며, Key와 매핑되는 Value를 가지는 경우 사용한다.

### Usage

```typescript
type userType = {
  [key: string]: string;
};

let user: userType = {
  홍길동: '사람',
  둘리: '공룡',
};

// Key의 타입은 string이며 Value의 타입은 string, number, boolean인 경우
type userType = {
  [key: string]: string | number | boolean;
};

let user: userType = {
  name: '홍길동',
  age: 20,
  man: true,
};
```

{% endhint %}

**타입과 인터페이스의 차이점**

- 유니온 타입은 있지만 유니온 인터페이스라는 개념은 없다.

  - 인터페이스는 타입을 확장할 수 있지만, 유니온은 할 수 없다.
  - 유니온 타입을 확장하려면

    1. 하나의 변수명으로 매핑하는 인터페이스를 만들어야 한다.

       ```typescript
       type Input = {
         /*...*/
       };
       type Output = {
         /*...*/
       };
       interface VariableMap {
         [name: string]: Input | Output;
       }
       ```

    2. 유니온 타입에 name 속성을 붙인 타입을 만들어야 한다.

       ```typescript
       type NamedVariable = (Input | Output) & { name: string };
       ```

- type 키워드를 통해 튜플과 배열 타입을 간결하게 표현할 수 있다.

  ```typescript
  type Pair = [number, number];
  type StriingList = string[];
  type NamedNums = [string, ...number[]];
  ```

- 인터페이스는 보강이 가능하다. (타입에는 없는 기능)

  ```typescript
  interface IState {
    name: string;
    capital: string;
  }

  interface IState {
    population: number;
  }

  const wyoming: IState = {
    name: 'Wyoming',
    capital: 'Cheyenne',
    population: 500_000,
  }; // ok
  ```

  - 속성을 확장하는 것을 **_선언 병합_**이라고 한다.
  - 주로 타입 선언 파일에서 사용된다.

_<mark style="color:red;">**타입과 인터페이스 중 어느 것을 사용해야할까?**</mark>_

- 타입이 복잡하다면 타입별칭을 사용하자.
- 일관된 스타일을 사용하자.(타입을 사용하고 있다면 일관되게 타입을 사용할 것.)
- 스타일이 확립되지 않은 프로젝트라면, 보강의 가능성이 있을 지 생각하자.

---

### 타입 연산과 제너릭 사용으로 반복 줄이기

> 타입 중복은 코드 중복만큼 많은 문제를 발생시킨다.

- 상수를 사용해서 반복을 줄이기

  ```typescript
  function distance(a: { x: number; y: number }, b: { x: number; y: number }) {
    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
  }

  // 중복 제거 코드
  interface Point2D {
    x: number;
    y: number;
  }

  function distance(a: Point2D, b: Point2D) {
    /*...*/
  }
  ```

- 전체 애플리케이션의 상태를 표현하는 타입의 부분집합으로 타입 만들기

  ```typescript
  interface State {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
    pageContens: string;
  }

  interface TopNavState {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
  }
  ```

  - 이 경우 TopNavState를 확장하여 State를 구성하는 것보다 State의 부분 집합으로 TopNavState를 정의하는 것이 바람직하다.
  - State를 인덱싱하여 속성의 타입에서 중복을 제거할 수 있다.

  ```typescript
  type TopNavState = {
    userId: State['userId'];
    pageTitle: State['pageTitle'];
    recentFiles: State['recentFiles'];
  };
  ```

- '매핑된 타입'을 사용하면 반복되는 코드를 제거할 수 있다.

  ```typescript
  type TopNavState = {
    [k in 'userId' | 'pageTitle' | 'recentFiles']: State[k];
  };

  type TopNavState = Pick<State, 'userId' | 'pageTitle' | 'recentFiles'>;
  ```

  - '매핑된 타입'은 배열의 필드를 루프 도는 것과 같은 방식이다. 👉🏻 Pick

{% hint="info"%}

### Pick

Pick은 T와 K 두 가지 타입을 받아서 결과 타입을 반환한다.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  brand: string;
  stock: number;
}

// 상품 목록을 받아오기 위한 api
function fetchProduct(): Promise<Product[]> {
  // ... id, name, price, brand, stock 모두를 써야함
}

type shoppingItem = Pick<Product, 'id' | 'name' | 'price'>;

// 상품의 상세정보 (Product의 일부 속성만 가져온다)
function displayProductDetail(shoppingItem: shoppingItem) {
  // id, name, price의 일부만 사용 or 별도의 속성이 추가되는 경우가 있음
  // 인터페이스의 모양이 달라질 수 있음
}
```

{% endhint %}


---

### 동적 데이터에 인덱스 시그니처 사용하기

> *자바스크립트의 장점 중 하나는 객체를 생성하는 문법이 간단하다는 것이다. 자바스크립트 객체는 문자열 키를 타입의 값에 관계없이 매핑핸다. '인덱스 시그니처'를 명시하여 유연하게 매핑을 표현할 수 있다.*


```typescript
type Rocket = {[property: string]: string};

const rocket: Rocket = {
  name: 'Falcon9',
  variant: 'v1.0',
  thrust_kN: '4,940 kN',
} // ok
```
- `[property: string]: string`이 인덱스 시그니처
  - 키의 이름: 키의 위치만 표시하는 용도, 타입체커에선 사용하지 않는다.
  - 키의 타입: string이나 number 또는 symbol의 조합이어야 하지만 보통은 string을 사용한다. 
  - 값의 타입: 어떤 것이든 될 수 있다. 

**이렇게 타입 체크가 수행되면 네 가지 단점이 드러난다.** 
1. 잘못된 키를 포함해 모든 키를 허용한다. 
2. 특정 키가 필요하지 않다. 
3. 키마다 다른 타입을 가질 수 없다. 
4. name: 을 입력할 때, 키는 무엇이든 가능하기 때문에 자동 완성 기능이 동작하지 않는다. 

_<mark style="color:red;">👉🏻 결론은 인덱스 시그니처는 부정확하므로 더 나은 방법을 찾아야한다는 것</mark>_

```typescript
interface Rocket {
  name: string;
  variant: string;
  thrust_kN: number;
}

const falconHeavy: Rocket = {
  name: 'Falcon Heavy',
  variant: 'v1',
  thrust_kN: 15_200
}
```

<mark style="color:red;">**인덱스 시그니처는 동적 데이터를 표현할 때 사용한다.**</mark>

예를 들어, CSV 파일처럼 헤더 행에 열 이름이 있고, 데이터 행을 열 일므과 값으로 매핑하는 객체로 나타내고 싶은 경우이다. 

{% hint style="info" %}
### CSV 파일이란?


- [CSV 파일형식](https://ko.wikipedia.org/wiki/CSV_(%ED%8C%8C%EC%9D%BC_%ED%98%95%EC%8B%9D))
- 몇 가지 필드를 쉼표로 구분한 텍스트 데이터 및 텍스트 파일
- 확장자는 .csv이며 MIME 형식은 text/csv이다. comma-separated variables라고도 한다.


<figure><img src="../../.gitbook/assets/230906-1.png" alt=""><figcaption></figcaption></figure>

위의 데이터 표는 다음과 같이 CSV 형식으로 표현할 수 있다:

```text
연도,제조사,모델,설명,가격
1997,Ford,E350,"ac, abs, moon",3000.00
1999,Chevy,"Venture ""Extended Edition""","",4900.00
1999,Chevy,"Venture ""Extended Edition, Very Large""",,5000.00
1996,Jeep,Grand Cherokee,"MUST SELL!air, moon roof, loaded",4799.00
```

{% endhint %}



```typescript
function parserCSV(input: string): {[columnName: string]: string}[] {
  const lines = input.split('\n');
  const [header, ...rows] = lines;
  const headerColumns = header.split(',');

  return rows.map(rowStr => {
    const row: {[columnNamae: string]: string} = {};
    rowStr.split(',').forEach((cell, i) => {
      row[headerColumns[i]] = cell;
    });
    return row;
  });
}
```

- 일반적인 상황에선 열 이름이 무엇인지 알 수 없기 때문에 인덱스 시그니처를 사용한다. 
- 반면에 열 이름을 알고 있는 특정환 상황에서 parseCSV가 사용된다면, 미리 선언해둔 타입으로 단언문을 사용한다. 
  ```typescript
    interface ProductRow {
      productId: string;
      name: string;
      price: string;
    }

    declare let csvData: string;
    const products = parseCSV(csvData) as unknown as ProductRow[];
  ```

**어떤 타입에 가능한 필드가 제한되어 있는 경우라면 인덱스 시그니처로 모델링하지 말아야한다.** 

예를 들어 데이터에 A, B, C, D 같은 키가 있지만 얼마나 많이 있는지 모른다면 선택적 필드 또는 유니온 타입으로 모델링한다. 

```typescript
interface Row1 { [column: string]: number } // 너무 광범위함
interface Row2 { a: number; b?: number; c?: number; d?: number } // 최선
type Row3 = {
  { a: number; } | { a: number; b: number; } | { a: number; b: number; c: number; } | { a: number; b: number; c: number; d: number}
} // 가장 정확하지만 사용하기 번거로움
```

또 다른 방법으로는 
1. Record를 사용하기
    - Record는 키 타입에 유연성을 제공하는 제너릭 타입
    ```typescript
      type Vec3D = Record<'x' | 'y' | 'z', number>;
      // Type Vec3D = {
      //  x: number;
      //  y: number;
      //  z: number;
      // }
    ```
2. 매핑된 타입을 사용하는 방법
    - 키마다 별도의 타입을 사용하게 해준다. 
    ```typescript
      type Vec3D = {[k in 'x' | 'y' | 'z']: number};
      // Type Vec3D = {
      //  x: number;
      //  y: number;
      //  z: number;
      // }

      type ABC = {[k in 'a' | 'b' | 'c']: k extends 'b' ? stirng: number}
      // Type ABC = {
      //  a: number;
      //  b: string;
      //  c: number;
      // }
    ```  

**요약**
- 런타임 때까지 객체의 속성을 알 수 없을 경우에만 인덱스 시그니처를 사용하도록 한다. 
- 안전한 접근을 위해 인덱스 시그니처 값 타입에 undefined를 추가하는 것을 고려해야한다. 
- 가능하다면 인터페이스, Record, 매핑된 타입 같은 정확한 타입을 사용하는 것이 좋다. 


---

### number 인덱스 시그니처보다는 Array, 튜플, ArrayLike 사용하기


자바스크립트에서 객체란 키/값 쌍의 모음이다. 키는 보통 문자열이며, 값은 어떤 것이든 될 수 있다. 
- 자바스크립트는 복잡한 객체를 키로 사용하려고 하면, toString 메서드가 호출되어 객체가 문자열로 변환된다. 
  ```js
  > x = {}
  > x[[1, 2, 3]] = 2
  > x
  { '1, 2, 3': 1 }
  ```

- 숫자는 키로 사용할 수 없다. 숫자를 사용하려고 하면 런타임은 문자열로 변환한다. 
  ```js
  > { 1: 2, 3: 4 }
  {'1': 2, '3': 4}
  ```


배열은 분명히 객체이다.
- 숫자 인덱스를 사용하는 것이 당연하다. 
  ```js
  > x = [1, 2, 3]
  > x[0]
  1
  ```
  - 앞의 인덱스들은 문자열로 변환되어 사용된다. _**즉, 문자열 키를 사용해도 배열의 요소에 접근할 수 있다.**_
    ```js
    > x['1']
    2
    ```
  - Object.keys를 이용해 키를 나열해보면 키가 문자열로 출력된다. 
    ```js
    > Object.keys(x)
    ['0', '1', '2']
    ```

타입스크립트는 이러한 혼란을 바로잡기 위해 숫자 키를 허용하고, 문자열 키와 다른 것으로 인식한다. 
```typescript
interface Array<T> {
  // ...
  [n: number]: T;
}
```

타입 체크 시점에 오류를 잡을 수 있어 유용하다. 
```typescript
const xs = [1, 2, 3];
const x0 = xs[0]; // ok
const x1 = s['1']; 
// Element implicitly has an 'any' type because index expression is not of type 'number'
```

어떤 길이를 가지는 배열과 비슷한 형태의 튜플을 사용하고 싶다면 ArrayLike 타입을 사용한다. 
```typescript
function checkedAccess<T>(xs: ArrayLike<T>, i: number): T {
  if(i < xs.length) {
    return xs[i];
  }

  throw new Error(`배열의 끝을 지나서 ${i}를 접근하려고 했습니다.`)
}
```

**요약**
- 배열은 객체이므로 키는 숫자가 아니라 문자열이다. 인덱스 시그니처로 사용된 number 타입은 버그를 잡기 위한 순수 타입스크립트 코드이다. 
- 인덱스 시그니처에 number를 사용하기 보다 Array나 튜플, ArrayLike 타입을 사용하는 것이 좋다. 


---

### 변경 관련된 오류 방지를 위해 readonly 사용하기

삼각수(1, 1+2, 1+2+3, ...)를 출력하는 코드
```typescript
function printTriangles(n: number) {
  const nums = [];
  for(let i = 0; i < n; i++) {
    nums.push(i);
    console.log(arraySum(nums));
  }
}

// 배열 안의 숫자들을 모두 합치는 함수
function arraySum(arr: number[]) {
  let sum = 0, num;

  while((num = arr.pop()) !== undefined) {
    sum += num;
  }

  return sum;
}
```

readonly 접근 제언자를 사용해 오류의 범위를 좁히기 위해 arraySum이 배열을 변경하지 않는다는 선언을 할 수 있다. 
```typescript
function arraySum(arr: readonly number[]) {
  let sum = 0, num;
  while((num = arr.pop()) !== undefined) { // Property 'pop' does not exist on type 'readonly number[]'.
    sum += num;
  }
  return sum;
}
```
readonly number[]는 '타입'이고, number[]와 구분되는 특징이 있다. 
- 배열의 요소를 읽을 수 있지만, 쓸 수는 없다.
- length를 읽을 수 있지만, 바꿀 수는 없다.
- 배열을 변경하는 pop을 비롯한 다른 메서드를 호출할 수 없다. 

**배개변수를 readonly로 선언하면 발생하는 일**
- 타입스크립트는 매개변수가 함수 내에서 변경이 일어나는지 체크한다. 
- 호출하는 쪽에서는 함수가 매개변수를 변경하지 않는다는 보장을 받는다.
- 호출하는 쪽에서 함수에 readonly 배열을 매개변수로 넣을 수도 있다. 


{% hint style="success" %}

### const와 readonly의 차이점

- const: 재 할당을 방지한다. (런타임 동작)
- readonly: 객체 내 변조를 막는다. (타입 스크립트 타입 체커에서 걸러줌)

{% endhint %}

