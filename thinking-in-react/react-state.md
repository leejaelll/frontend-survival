# React State

## 📚 강의 정리

- step 3. Find the minimal but complete representation of UI state  
  : 최소한이지만 UI 상태 표현 완성하기
- step 4. Identify where your state should live  
  : 상태가 어디서 살아있어야하는지 식별하기
- step 5. Add inverse data flow  
  : 역데이터 흐름 추가하기

## React의 State

React의 state는 “변경”을 다루기 위한 요소. “Re-rendering”이란 주제에서 다뤄진다. 거칠게 이야기하면, 어떤 컴포넌트의 state가 바뀌면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하게 된다.

아무렇게나 막 만들어도 되지만, 일관성과 효율을 위해 **DRY 원칙을 따르는 SSOT**를 만든다.

- [DRY (Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/중복배제)
- [SSOT (Single Source of Truth)](https://ko.wikipedia.org/wiki/단일_진실_공급원)

---

{% hint style="info" %}

React State의 조건:

1. 변경돼야 함. 변경되지 않는 건 state로 다룰 가치가 없다.

2. 부모 컴포넌트가 props를 통해 전달한다면 state가 아님.

3. 다른 state나 props를 이용해 계산 가능하다면 state가 아님.

{% endhint %}

예제 애플리케이션의 모든 데이터 조각들을 생각해보기

1. 원래 제품 목록ㄴ
2. 사용자가 입력한 검색 텍스트
3. 체크박스의 값
4. 필터링된 제품 목록

여기서 상태는 무엇인가? 상태가 아닌 것들을 판별해보자.

- 시간이 지나도 변하지 않는가? 👉🏻 상태 X
- 부모로부터 props를 통해 전달받는가? 👉🏻 상태 X
- 컴포넌트의 기존 state나 props를 기반으로 계산할 수 있는가? 👉🏻 상태 X

이 외의 나머지는 모두 상태이다.

예제 데이터를 보면서 살펴보자.

1. 원래 제품 목록: props로 전달되었기 때문에 상태 X
2. 사용자가 입력한 검색 텍스트: 시간이 지남에 따라 변경되고 아무것도 계산할 수 없으므로 상태 O
3. 체크박스의 값: 시간이 지남에 따라 변경되고 계산이 어려우므로 상태 O
4. 필터링된 제품 목록: 원래 제품 목록을 가져와서 검색 텍스트 및 확인란 값에 따라 필터링하여 계산할 수 있으므로 상태 X

---

다루는 상태가 너무 많으면 복잡함. TypeScript를 잘 쓰면 직접 관리하는 상태의 수를 줄여줄 수 있다.

그렇다면 그 상태를 누가 관리해야 할까? 더 정확히는, 상태를 누가 소유해야 할까?

(React만 쓴다면) 해당 상태에 의존적인 컴포넌트를 모두 포함하는 컴포넌트가 상태를 소유해야 함.

- [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)
- [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

---

## Inverse Data Flow

하위 컴포넌트의 props로 함수를 전달. 흔히 콜백 함수라고 부름.

TypeScript(정확히는 JavaScript)는 함수가 일급(first-class) 객체다. 즉, 어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용할 수 있다. 익명 함수, 클로저 등과 함께 사용하면 시너지가 큼.

- [일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)
- 다시 [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)

---

## 참고

아이템 목록에서 key가 value인 것만 선택하기.

```jsx
function select<ItemType, ValueType>(
	items: ItemType[],
	key: keyof ItemType,
	value: ValueType,
) {
	return items.filter((item) => item[key] === value);
}
```

---

## ✅ Keyword

## React state란?

- React에서 state란 app이 기억해야하는 최소한의 변경 데이터 집합을 의미한다.
- 상태를 구조화할 때 가장 중요한 원칙은 반복하지 않는 것이다. 👉🏻 DRY 원칙
- State의 조건
  - props으로 전달받지 않은 것 (상태O)
  - 시간이 지나면 변경되는 것 (상태O)
  - 컴포넌트의 기존 state나 props를 기반으로 계산할 수 있는가? (상태X)

## DRY 원칙

- Don't Repeat Yourself
- 소프트웨어 공학에서 사용되는 개발 원칙 중 하나로, 중복 코드를 최소화하여 코드의 유지 보수성, 재사용성 및 확장성을 높이기 위해 코드를 작성할 때 사용
- DRY 원칙을 지키는 방법
  - 중복 코드는 피한다.
  - 비즈니스 로직과 같은 핵심 기능은 한 번만 작성한다.
  - 공통으로 사용되는 코드는 모듈화하여 재사용성을 높인다.
  - 데이터 구조와 알고리즘을 복사하지 말고, 재사용 가능한 모듈로 분리한다.

## SSOT(Single Source of Truth)

- 소프트웨어 설계 및 데이터 관리에서 사용되는 개념으로, "단일 진실 공급원"이라고 부른다.
- 데이터의 일관성을 보장하고 중복을 제거하여 **데이터의 일관성을 유지**하려는 목적으로 사용한다.
- SSOT는 다중 데이터 소스 및 다중 시스템의 데이터 복제 및 분산을 방지하여 데이터를 중앙 집중화
  - 모든 데이터 요소를 한 곳에서만 제어, 편집하도록 조직하는 형태를 의미한다.
  - 데이터의 정합성과 일관성을 보장하고, 데이터 업데이트 및 변경을 용이하게 한다.

## useState

- 컴포넌트에 상태 변수를 추가할 수 있는 React Hook

```jsx
const [state, setState] = useState(initialState);

// usage

import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());
  // ...
```

- 배열 디스트럭처링을 이용해 [something, setSomething]처럼 쓰는 것이 일반적

Parameters

- initialState: 처음에 상태를 설정할 값
  - 모든 유형의 값이 올 수 있다.
  - 이 인수는 초기 렌더링 이후에 무시된다.
- 함수를 인수로 전달하면 초기화 함수로 취급된다.
  - 이 함수는 순수해야 하고 인수를 받지 않아야 하며 값을 반환해야 함
  - 컴포넌트를 초기화 할 때 이 함수를 호출하고 그 반환값을 초기 상태로 저장

## 1급 객체(first-class object)란?

- 다음과 같은 조건을 만족하는 객체를 일급 객체라고 한다.
  1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  2. 변수나 객체, 배열 등에 저장할 수 있다.
  3. 함수의 매개변수에 전달할 수 있다.
  4. 함수의 반환값으로 사용할 수 있다.

{% hint style="success" %}

함수는 위의 조건을 모두 만족하므로 일급 객체이다.

```jsx
// 1. 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.

const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수는 매개변수에 전달할 수 있다.
// 4. 함수는 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };

  const increaser = makeCounter(auxs.increase);
  const decreaser = makeCounter(auxs.decrease);
  console.log(increaser()); // 1
  console.log(increaser()); // 2
  console.log(decreaser()); // -1
}
```

{% endhint %}

> 함수가 일급객체라는 것은 함수를 객체와 동일하게 사용할 수있다는 의미

- 객체는 값이므로 함수는 값과 동일하게 취급할 수 있다.
- 따라서 함수는 값을 사용할 수 있는 곳이라면 어디든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.
- 일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수 있다는 것 👉🏻 함수형 프로그래밍을 가능케 하는 자바스크립트 장점 중 하나

함수와 객체의 차이점

- 일반 객체는 호출할 수 없지만 함수는 호출이 가능하다.
- 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

## Lifting State Up

- 컴포넌트 간에 상태를 공유하기 위해 상위 컴포넌트로 상태를 끌어올리는 것을 의미
- 상태를 공유하기 위해 여러 하위 컴포넌트에서 동일한 상태를 사용해야 하는 경우, 각 하위 컴포넌트가 동일한 상태를 중복으로 유지하는 것은 비효율적
- 이 때, 상위 컴포넌트에서 상태를 유지 👉🏻 하위 컴포넌트는 props를 통해 상태를 전달받아 사용하는 것을 Lifting State Up이라고 함

---
