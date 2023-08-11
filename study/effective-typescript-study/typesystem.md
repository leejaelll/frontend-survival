# 타입스크립트의 타입시스템

> _**타입스크립트의 가장 중요한 역할은 타입시스템에 있다.**_&#x20;

| 타입시스템이란 무엇인가?                  |
| ----------------------------------------- |
| 타입 시스템을 어떻게 사용해야할까?        |
| 무엇을 결정해야하는가?                    |
| 가급적 사용하지 말아야할 기능은 무엇일까? |

### 타입이 값들의 집합이라고 생각하기

{% hint style="info" %}

#### never 타입은 언제 사용하는 걸까?

- 아무 값도 포함하지 않는 **공집합**
- never 타입으로 선언된 변수에는 아무런 값도 할당할 수 없다.

```typescript
function hello(): never {
  throw new Error('Error');
}
```

_never는 따로 return하지 않고, 오류를 발생시키는 함수에 타입을 지정할 때 사용한다._

#### unknown

- 변수의 타입을 모를때 unknown으로 지정할 수 있다.

```typescript
let a: unknown;
if (typeof a === number) {
  let b = a + 1;
}
```

any와 unknown를 분리해놓은 이유가 무엇일까? 이 둘의 차이점은 무엇일까?
any와 unknwon 둘 다 모든 타입을 허용하는 것은 동일하다. 하지만 any를 사용하게 되면 타입검사를 느슨하게 하므로 개발과정에서 오류를 발생시키지 않는다. 반면 unknown의 경우에는 프로퍼티를 사용하거나 연산을 하게 될 경우, 타입체크를 하게된다. 그러므로 문제가 되는 코드를 미리 예방할 수 있다.

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

> 타입 연산자는 인터페이스의 속성이 아닌, 값의 집합에 적용된다.

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

#### typeof / keyof의 차이점

#### **`typeof`**

: 데이터를 데이터 타입으로 변환해주는 연산자

```typescript
let str1 = 'hello';
let str2: typeof str = 'hi';
// === let str2: string ="hi"
```

#### **`keyof`**

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
