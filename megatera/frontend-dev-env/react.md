# React

### 📚 강의 정리

[React 공식 문서](https://ko.reactjs.org/)

* Beta 버전 문서 → [https://beta.reactjs.org/](https://beta.reactjs.org/)
* 기존엔 클래스형 컴포넌트를 기준으로 설명했다면 최근 베타문서는 함수형 컴포넌트를 기준으로 설명
* 리액트를 처음 시작하는 거라면 베타 문서를 먼저 읽는 것을 권장

#### _리액트에서 제일 중요한 것은 상태(state)_

상태를 골라내는 게 핵심이다.

[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react) 를 참고해서 리액트에서 말하는 상태에 관한 개념에 대해서 공부하자. 👉🏻 [Thinking in React 정리 🦖](https://jazzy-sneezeweed-14f.notion.site/Thinking-in-React-3dc21d2691dd4302957bed94a7e7b582)

상태가 변경되면 자동으로 렌더링이 된다.

\


### 렌더링

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

* `id = “root”` 를 가진 노드가 없으면 return 시키고, 있다면 해당 노드를 가지고 createRoot를 한다.
* root에 `<Greeting />` 컴포넌트를 렌더링 하는 것

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

* 1초마다 count가 증가함
* 이때 DOM 전체를 리렌더링하는 것이 아니라, 필요한 부분만 업데이트 시키는 것 👉🏻 **리액트의 렌더링 방식**
* [updating-a-root-component](https://beta.reactjs.org/reference/react-dom/client/createRoot#updating-a-root-component)

\


### 리렌더링

리액트는 언제 리렌더링을 하는가?

간단히 말하면 리액트는 state를 가지고 있다. state가 변경되었을 때 리렌더링을 한다.

* [React는 컴포넌트를 언제 다시 리렌더링 할까요?](https://velog.io/@surim014/react-rerender)
* [왜 리액트에서 리렌더링이 발생하는가.](https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%99%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%B4-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%EA%B0%80-74dd239b0063)
* [React 렌더링 동작에 대한 (거의) 완벽한 가이드](https://velog.io/@superlipbalm/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior)

> 컴포넌트의 return문 안에서는 무조건 하나의 노드로만 나가야한다. 그래서 사용하는 것이 `React.fragment`. 일반적으로 `<></>` 로 사용한다.



### ✅ Keyword

#### React란?

* 페이스북에서 개발된 UI(User Interface) 라이브러리

🙋🏻‍♀️ 리액트가 왜 생겨나게 된걸까?

* 기존의 웹 개발 방식에서는 DOM(Document Object Model)을 직접 조작하여 UI를 처리함
* 이 방식은 대규모 데이터 처리 시 성능 이슈가 발생하는 단점이 있음
* 이러한 단점을 개선하고자 페이스북에서는 가상 DOM(Virtual DOM) 기반으로 작동하는 리액트를 개발

#### React 컴포넌트

🙋🏻 React Component란?

* UI를 구성하는 독립적인 모듈
* 컴포넌트는 입력값(props)을 받아들이고, 이를 기반으로 UI를 렌더링한다.
*   리액트 컴포넌트로 구성한 페이지의 구조는 다음과 같다.

    ```jsx
    <PageLayout>
      <NavigationHeader>
        <SearchBar />
        <Link to="/docs">Docs</Link>
      </NavigationHeader>
      <Sidebar />
      <PageContent>
        <TableOfContents />
        <DocumentationText />
      </PageContent>
    </PageLayout>
    ```

🙋🏻‍♂️ React Component를 사용하는 이유가 뭘까?

* 프로젝트 규모가 커질수록 재사용하는 코드가 많아진다.
* 이때 리액트 컴포넌트를 사용하면 이미 작성한 컴포넌트를 재사용하여 많은 디자인을 구성할 수 있다.

🙋🏻 React Component를 정의하는 방법은?

```jsx
export default function Profile() {
  return <img src="https://i.imgur.com/MK3eW3Am.jpg" alt="Katherine Johnson" />;
}
```

#### React 리렌더링

🙋🏻 리액트는 어떻게 변경된 부분만 파악해서 리렌더링 성능을 최적화시킬까?

* 리액트에서는 가상 돔(Virtual DOM)을 사용하여 리렌더링 성능을 최적화 시킴
* 컴포넌트의 상태나 프로퍼티가 변경되면, 컴포넌트의 가상 돔을 업데이트
* 이때, 이전 가상 돔과 새로운 가상 돔을 비교하여 변경된 부분만을 실제 돔에 반영한다. 👉🏻 이 과정을 `재조정(Reconciliation)`이라고 함

🙋🏻‍♀️ 재조정(Reconciliation)이란?

* 이전 가상 돔과 새로운 가상 돔을 재귀적으로 비교하면서 변경된 부분 찾아내는 것을 의미함
* 리액트에서는 컴포넌트의 state나 props가 변경될 때마다 가상 DOM이 업데이트되고, 이후 재조정이 이루어지므로, 사용자가 상호작용할 때마다 매번 DOM을 조작하지 않아도 된다.

#### IoC(Inversion of Control)

* 제어의 역전이라는 뜻으로, 프로그래밍에서의 디자인 패턴 중 하나
*   보통의 프로그램은 개발자가 코드를 작성하고, 프로그램이 순차적으로 코드를 실행하는 방식

    * 이 경우, 프로그램의 제어 흐름은 개발자가 코드를 작성할 때 결정

    \


> 하지만 IoC에서는 프로그램의 제어 흐름이 개발자가 작성한 코드에 의해 결정되는 것이 아니라, 프로그램이 실행되는 도중에 프레임워크나 컨테이너가 제어를 가져가게 된다.

* 이렇게 되면 개발자가 코드를 작성할 때 결정한 제어 흐름이 아니라, 프레임워크나 컨테이너가 제어 흐름을 결정하게 되므로, 프로그램의 유연성과 확장성이 향상됨

🙋🏻‍♂️ 그렇다면 리액트도 IoC의 사례 중 하나일까?

* 리액트에서는 컴포넌트를 조합하여 UI를 구성한다.
* 이 때 개발자가 직접 컴포넌트의 생명주기를 관리하지 않고, 리액트 엔진이 컴포넌트의 생성, 업데이트, 소멸 등을 자동으로 처리한다.
* 그러므로 리액트도 IoC의 개념을 적용하고 있다고 볼 수 있다.

#### Library Vs. Framework

* Library와 Framework 모두 코드를 재사용할 수 있도록 도와주는 도구

> 둘의 차이는 누가 누구를 호출하는가에 달려있다.

📚 **Library**

* 개발자가 코드에서 필요한 기능을 호출하는 방식으로 사용된다.
* 즉, 개발자는 필요한 기능이 있을 때, 라이브러리를 호출하여 사용
* 예를 들어, jQuery는 웹페이지를 조작하기 위한 다양한 기능을 제공하는 라이브러리이다.

📚 **Framework**

* 개발자가 작성한 코드를 호출하는 방식으로 사용
* Framework는 특정한 문제를 해결하기 위해 필요한 코드를 개발자가 작성하는 것이 아니라, 개발자가 작성한 코드를 Framework가 호출하여 사용
* 즉, 개발자가 제공한 코드를 호출하는 것
* Framework는 보통 어플리케이션을 개발하는데 필요한 여러가지 기능과 규칙들을 제공
