# readonly 사용하기

## `readonly`

객체의 속성이나 배열의 요소를 읽기 전용으로 만들기 위해 사용한다. readonly로 선언된 속성은 초기화 후에는 변경할 수  없다. 👉🏻 _<mark style="color:red;">**데이터의 불변성을 보장할 때 유용**</mark>_

***

## 사용 예시

### 1. 인터페이스에서 readonly 사용

속성을 readonly로 선언하여 읽기 전용으로 만들 수 있다.&#x20;

```typescript
interface User {
  readonly id: number;
  name: string;
}

const user: User = { id: 1, name: "Alice" };

console.log(user.id); // 1

user.name = "Bob"; // OK
// user.id = 2; // Error: Cannot assign to 'id' because it is a read-only property.
```



### 2. 클래스에서  readonly 사용

class에서도 속성을 readonly로 선언할 수 있다. readonly 속성은 클래스 내부에서 초기화될 수 있지만, 이후에는 수정할 수 없다.&#x20;

```typescript
class Person {
  readonly id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}

const person = new Person(1, "John");

console.log(person.id); // 1

person.name = "Jane"; // OK
// person.id = 2; // Error: Cannot assign to 'id' because it is a read-only property.
```



### 3. 배열에서 readonly 사용

```typescript
const numbers: readonly number[] = [1, 2, 3, 4];

console.log(numbers[0]); // 1
```



### 4. Readonly 유틸리티 사용

TypeScript는 객체의 모든 속성을 읽기 전용으로 만드는 Readonly 유틸리티 타입을 제공한다.&#x20;

```typescript
interface User {
  id: number;
  name: string;
}

const user: Readonly<User> = { id: 1, name: "Alice" };

console.log(user.id); // 1

// user.name = "Bob"; // Error: Cannot assign to 'name' because it is a read-only property.
// user.id = 2; // Error: Cannot assign to 'id' because it is a read-only property.
```
