## 학습 키워드

- React Hook 이란
- Hooks
  - useState
  - useEffect
  - useContext
  - useRef
  - useLayoutEffect
- React StrictMode 란

---

## React의 Hook

- [Hook의 개요](https://ko.reactjs.org/docs/hooks-intro.html)

- [Hook 개요](https://ko.reactjs.org/docs/hooks-overview.html)

- [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html)

React 16.8에서 Hooks가 도입됨. 기존 방식에 있던 몇 가지 문제를 해결.

[React Conf 2018 Hooks 소개 영상](https://youtu.be/dpw9EHDh2bM)

<br />

기존 방식의 문제점

- Wrapper Hell (HoC)

- Huge Components

- Confusing Classes

<br />

🦖 1:54 - 고차 컴포넌트를 사용하는 방법

고차 구성 요소(HOC)는 여러 구성 요소에서 기능을 재사용할 수 있게 해주는 React의 유용한 패턴

기본 패턴

```jsx
import React from 'react';

export default function WithHoc(WrappedComponent) {
  return function WithHoc(props) {
    return <WrappedComponent {...props}>
  }
}
```

로딩 상태 출력 기능을 HOC로 구현하는 예제:

```jsx
import React from 'react';

export default function withLoading(WrappedComponent) {
  return function withLoading(props) {
    const {isLoading} = props

    if(isLoading) {
      return '로딩 중!'
    }
    return <WrappedComponent {...props}>
  }
}

```

여러 컴포넌트에 적용하는 방법:

```jsx
import React from 'react';
import withLoading from './withLoading';


function Text() {
  return '텍스트 메세지';
}

function Input() {
  return <Input />;
}

const TextwithLoading = withLoading(Text);
const InputwithLoading = withLoading(Input);

function App() {
  return (
    <div className="App">
      <TextwithLoading isLoading />
      <InputwithLoading isLoading />
    <div>
  )
}
```

<br />

[HoC (Higher-Order Components)](https://ko.reactjs.org/docs/higher-order-components.html)

컴포넌트의 재사용성을 높이기 위해서 컴포넌트를 컴포넌트로 감싸서 porps로 내려주는 스킬

복잡하고 컴포넌트가 계속 생기는 단점이 있다.

컴포넌트가 그 안에 로직이 많아지기 때문에 컴포넌트 자체가 커지는 문제가 있다.

{% hint style="info" %}

React를 쓰는 방식을 완전히 바꾼 커다란 변화.

→ 이제는 예전으로 돌아가는 게 불가능하다!

{% endhint %}

기존:

- 상태를 가진 컴포넌트는 Class Component로 만들고, props만 사용하는 재사용이 용이한 작은 컴포넌트는 Function Component로 작성.
- Redux에서도 비슷한 구분이 존재했다.
  - [Presentational and Container Components - Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

현재:

- 그냥 Function Component만 사용.
- 상태 관리 유무를 바로 알기 어려움 = 신경쓰지 않아도 됨.
- 복잡한 요소는 전부 Hook으로 격리 및 재사용 가능.

<br />

### 대표적인 Hooks

- useState → State Hook ⇒ React의 State
- useEffect ⇒ Side-effect
- useContext
- useRef
- useLayoutEffect → useEffect와 조금 다름.

<br />

## useEffect

- [Synchronizing with Effects](https://beta.reactjs.org/learn/synchronizing-with-effects)

- [You Might Not Need an Effect](https://beta.reactjs.org/learn/you-might-not-need-an-effect)

- [Using the Effect Hook](https://ko.reactjs.org/docs/hooks-effect.html)

- [useEffect](https://beta.reactjs.org/reference/react/useEffect)

- [useEffect 완벽 가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)

렌더링 이후 해야 할 일, 즉 React의 외부와 관련된 일을 정해줄 수 있다.

<br />

🦖 10:00 - 렌더링이 어떤 렌더링을 의미하는가?

화면이 렌더링되는 것을 의미하나?

정확하게 화면이 그리는 것을 의미하지는 않는다. 언제가 타이밍인지는 모름

DOM 객체를 참조해야할 때 DOM 트리가 모두 완성되고 나서 참조를 해야한다. 그냥 사용하면 null로 나오는 경우가 있는데 이때 useEffect를 사용하면 순서를 보장할 수 있다.

<br />

기본적으로 렌더링 때마다 실행되므로, 의존성 배열을 통해 언제 이펙트를 실행할지 지정할 수 있다(= 불필요한 경우에 건너뛸 수 있다).

document.title을 변경하는 예제

```jsx
export default function App() {
  document.title = '변경된 타이틀';

  return (
    <div>
      <h1>상품</h1>
    </div>
  );
}
```

이렇게 사용해도 문제가 없지만 useEffect를 사용하면 더 안전하다.

```jsx
export default function App() {
  useEffect(() => {
    document.title = '변경된 타이틀';
  });

  return (
    <div>
      <h1>상품</h1>
    </div>
  );
}
```

state를 가지고 있는 컴포넌트에서 사용하는 예제:

```jsx
import { useState, useEffect } from 'react';

export default function FilterableProductTable() {
  useEffect(() => {
    console.log('Effect!');
    document.title = '변경된 타이틀';
  });
  const [filterText, setFilterText] = useState('');
}
```

👉🏻 input창에 텍스트를 입력할 때마다 'Effect'가 계속해서 찍힘

### 타이머 예제

타이머를 on/off하는 기능 구현한다고 생각해보자.

```jsx
function Timer() {
  useEffect(() => {
    setInterval(() => {
      document.title = `Now: ${new Date().getTime()}`;
    }, 100);
  });

  return <p>Playing</p>;
}

export default function TimerControl() {
  const [playing, setPlaying] = useState(false);

  const handleClick = () => {
    setPlaying(!playing);
  };

  return (
    <div>
      {playing ? <Timer /> : <p>Stop</p>}
      <button type="button" onClick={handleClick}>
        Toggle
      </button>
    </div>
  );
}
```

이 코드의 문제점

- toggle 버튼을 누르면 title 숫자가 변경되는 것까지는 정상작동
- 다시 toggle 버튼을 누르면 off되지 않고 계속해서 title이 변경됨

해결 방법

- 함수를 리턴함으로써 종료 처리를 할 수 있다.

```jsx
useEffect(() => {
  const savedTitle = document.title;

  const id = setInterval(() => {
    document.title = `Now: ${new Date().getTime()}`;
  }, 100);

  return () => {
    document.title = savedTitle;
    clearInterval(id);
  };
});
```

<br />

### 처음에 한번만 실행하기

의존성 배열에서 아무 것도 지정하지 않으면 맨 처음에 딱 한번만 실행하게 할 수 있다.

처음에 한 번만 실행하는 경우 👉🏻 주로 API를 호출해서 데이터를 얻을 때 사용한다.

```jsx
const [products, setProducts] = useState<Product[]>([]);

useEffect(() => {
	const fetchProducts = async () => {
		const url = 'http://localhost:3000/products';
		const response = await fetch(url);
		const data = await response.json();
		setProducts(data.products);
	};

	fetchProducts();
}, []);
```

Fetch 함수의 위치가 고민된다면, Dan Abramov의 글을 다시 보자.

- [useEffect 완벽가이드 - 함수를 이펙트 안으로 옮기기](https://overreacted.io/ko/a-complete-guide-to-useeffect/#%ED%95%A8%EC%88%98%EB%A5%BC-%EC%9D%B4%ED%8E%99%ED%8A%B8-%EC%95%88%EC%9C%BC%EB%A1%9C-%EC%98%AE%EA%B8%B0%EA%B8%B0)

<br />

### Effect가 두 번 실행되는 문제

`<React.StrictMode>`로 컴포넌트 전체를 감쌀 경우, 예상치 못한 Side Effect를 찾으려고 Effect 등을 **두 번씩 실행함**. 평소에는 큰 문제가 없지만, API 등을 사용하면 이상하다고 느낄 수 있으니 참고할 것.

- [예상치 못한 부작용 검사](https://ko.reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)

### 의존성 배열을 이용해 Fetch할 때 주의사항

- [Fetching data](https://beta.reactjs.org/learn/synchronizing-with-effects#fetching-data)
