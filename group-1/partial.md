# Partial 활용하기

## `Partial<T>`

Partial는 제네릭 타입으로, 타입 T의 모든 속성을 선택적으로 만든다.&#x20;

***

## Partial의 타입 사용 예시

### 1. 속성 optional로 만들기

User의 타입을 지정했다고 가정해보자.&#x20;

```typescript
interface User {
  id: string;
  email: number;
  phoneNum: number;
}
```



이때 phoneNum이 없는 사용자가 있다면 이 때 Partial을 사용해 타입의 속성을 선택적으로 사용할 수 있다.&#x20;

```typescript
const userWithPhone: Partial<User> ={
  id: 1,
  email: 'someEmail@email.com'
}
```



### 2. 함수의 매개변수로 사용

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description?: string;
}

const updateProduct = (product: Product, updates: Partial<Product>): Product => {
  return { ...product, ...updates };
};

let product: Product = { id: 1, name: "Laptop", price: 1000 };

// price만 업데이트
product = updateProduct(product, { price: 900 });

console.log(product);
```



### 3. 객체의 부분 업데이트

```typescript
interface Profile {
  username: string;
  bio: string;
  website?: string;
}

// 기존 Profile 타입을 Partial을 사용하여 모든 속성을 optional로 만든다
type PartialProfile = Partial<Profile>;

const updateProfile = (profile: Profile, updates: Partial<Profile>): Profile => {
  return { ...profile, ...updates };
};

let profile: Profile = { username: "john_doe", bio: "Web Developer" };

// bio와 website만 업데이트
profile = updateProfile(profile, { bio: "Full Stack Developer", website: "https://example.com" });

console.log(profile);
```
