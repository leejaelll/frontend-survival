# Polymorphic 컴포넌트를 사용한 유연한 UI 패턴 만들기

## 요구사항

요구사항은 다음과 같다.&#x20;

1. Text 컴포넌트는 as (prop)을 통해 다양한 태그를 렌더링할 수 있어야 한다.&#x20;
2. as prop의 기본 값은 span으로 지정한다.&#x20;
3. as prop의 값에 따라 다른 태그를 렌더링할 수 있어야 한다.&#x20;



## 결과

사용했을 때 결과값은 다음과 같다.&#x20;

```typescript
const Text = () => {};

export default function Index() {
  return <>
  <Text as="h1">Heading1 Text</Text> -> <h1>Heading1 Text</h1>
  <Text as="div">Div Text</Text> -> <div>Div Text</div>
  <Text>span text</Text> -> <span>span text</span>
  </>;
}
```

***

## 구현

### 첫번째 구현

```typescript
const Text = ({ as, children }) => {
  const Component = as || 'span';

  return <Component>{children}</Component>;
};

export default function Index() {
  return (
    <>
      <Text as='h1'>Heading1 Text</Text>
      <Text as='div'>Div Text</Text>
      <Text>span text</Text>
    </>
  );
}
```

#### 코드의 문제점

1. as의 값을 지정해줘야하는데 어떤 값이 들어올지 예측할 수 없다. 👉🏻 _**제네릭 사용**_
2. 추가적으로 prop을 전달했을 때 타입 오류를 캐치하지 못한다.&#x20;



### 두번째 구현

제네릭을 사용하여 props 타입을 지정한다.&#x20;

```typescript
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
};
```



우리가 전달하는 element에 따라서 element의 attributes도 함께 전달하고 사용할 수 있어야 한다.&#x20;

```typescript
type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, 'as' | 'children'>;
```



Text 컴포넌트의 타입을 지정해준다.&#x20;

```typescript
const Text = <C extends React.ElementType>({ as, children, ...rest }: TextProps<C>) => {
  const Component = as || 'span';
  return <Component {...rest}>{children}</Component>;
};
```

```typescript
import React from 'react';

type TextProps<T extends React.ElementType> = {
  as?: T;
  children: React.ReactNode;
} & Omit<React.ComponentPropsWithoutRef<T>, 'as' | 'children'>;

const Text = <C extends React.ElementType>({ as, children, ...rest }: TextProps<C>) => {
  const Component = as || 'span';
  return <Component {...rest}>{children}</Component>;
};

export default function Index() {
  return (
    <>
      <Text as='h1'>Heading1 Text</Text>
      <Text as='div'>Div Text</Text>
      <Text as='a' href='www.naver.com'>
        Link Text
      </Text>
      <Text>span text</Text>
    </>
  );
}

```
