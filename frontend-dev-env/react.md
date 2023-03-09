## 📚 강의 정리

[React 공식 문서](https://ko.reactjs.org/)

- Beta 버전 문서 → [https://beta.reactjs.org/](https://beta.reactjs.org/)
- 기존엔 클래스형 컴포넌트를 기준으로 설명했다면 최근 베타문서는 함수형 컴포넌트를 기준으로 설명
- 리액트를 처음 시작하는 거라면 베타 문서를 먼저 읽는 것을 권장

### _리액트에서 제일 중요한 것은 상태(state)_

상태를 골라내는 게 핵심이다.

[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react) 를 참고해서 리액트에서 말하는 상태에 관한 개념에 대해서 공부하자.

상태가 변경되면 자동으로 렌더링이 된다.

 <br>

## 렌더링

```jsx
function Greeting() {
  return <p>Hello, world!</p>;
}

function main() {
  const element = document.getElementById('root');

  if (!element) {
    return;
  }

  const root = ReactDOM.createRoot(element);

  root.render(<Greeting />);
}

main();
```

- `id = “root”` 를 가진 노드가 없으면 return 시키고, 있다면 해당 노드를 가지고 createRoot를 한다.
- root에 `<Greeting />` 컴포넌트를 렌더링 하는 것

여러 개의 Root를 만들거나, 여러 번 render해도 된다.

```jsx
import ReactDOM from 'react-dom/client';
import App from './App';

function Demo({ count }: { count: number }) {
  return <p>Demo : {count} </p>;
}

function main() {
  const element = document.getElementById('root');
  const element2 = document.getElementById('demo');

  if (!element || !element2) {
    return;
  }

  const root = ReactDOM.createRoot(element);
  const root2 = ReactDOM.createRoot(element2);

  root.render(<App />);
  root2.render(<Demo />);

  let count = 0;
  setInterval(() => {
    count += 1;
    root2.render(<Demo />);
  }, 1_000);
}

main();
```

- 1초마다 count가 증가함
- 이때 DOM 전체를 리렌더링하는 것이 아니라, 필요한 부분만 업데이트 시키는 것 👉🏻 **리액트의 렌더링 방식**
- [updating-a-root-component](https://beta.reactjs.org/reference/react-dom/client/createRoot#updating-a-root-component)

<br>

## 리렌더링

리액트는 언제 리렌더링을 하는가?

간단히 말하면 리액트는 state를 가지고 있다. state가 변경되었을 때 리렌더링을 한다.

- [React는 컴포넌트를 언제 다시 리렌더링 할까요?](https://velog.io/@surim014/react-rerender)
- [왜 리액트에서 리렌더링이 발생하는가.](https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%99%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%B4-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%EA%B0%80-74dd239b0063)
- [React 렌더링 동작에 대한 (거의) 완벽한 가이드](https://velog.io/@superlipbalm/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior)

> 컴포넌트의 return문 안에서는 무조건 하나의 노드로만 나가야한다. 그래서 사용하는 것이 `React.fragment`. 일반적으로 `<></>` 로 사용한다.

---

## ✅ Keyword

### React란?

### React 컴포넌트

### React 리렌더링

### IoC(Inversion of Control)

### Library vs Framework

---

## 🐋 Supplement
