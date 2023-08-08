# Routes

## 학습 키워드

* 라우터란?
* React Router
  * Browser Router
  * Route
  * Memory Router



### 🫥 라우터란 무엇인가?

\


## React Router

리액트 라우터가 없었을 때 어떻게 컴포넌트를 렌더링했지?

```jsx
export default function App() {
  const path = window.location.pathname;

  return (
    <div>
      <Header />
      <main>
        {path === '/' && <HomePage />}
        {path === '/about' && <AboutPage />}
      </main>
      <Footer />
    </div>
  );
}
```

리액트 자체에는 URL에 맞게 컴포넌트를 렌더링할 수 있는 기능이 없기 때문에 이렇게 사용할 수 밖에 없다.

React SPA에서는 라우팅을 위해 React Router라는 라이브러리를 사용한다.

* [React Router](https://reactrouter.com/)

> BrowserRouter, Routes, Route 를 주로 사용한다.

* `BrowserRouter` : HTML5 History API를 사용하여 페이지를 새로고침하지 않고도 주소를 변경할 수 있게 해준다.
* `Routes`: 경로를 매칭해주는 역할을 한다. 여러 Route를 감싸서 경로가 일치하는 컴포넌트를 렌더링한다.
* `Route`: 해당 path에서 어떤 컴포넌트를 렌더링할 것인가를 정한다.

\


### React Router 실습해보기

```jsx
// App.tsx
import { Routes, Route } from 'react-router-dom';

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
    </Routes>
  );
}
```

Routes와 Route를 사용하려면 BrowserRouter로 감싸야한다.

```jsx
// main.tsx
<BrowserRouter>
  <App />
</BrowserRouter>
```

\


### 🫥 테스트 환경에서는 어떻게 해야할까? 👉🏻 `MemoryRouter`

{% hint style="info" %}
#### MemoryRouter

React.js에서 사용되는 라우팅 방식 중 하나로 BrowserRouter와 비슷한 방식으로 동작한다. 하지만 브라우저에서 동작하는 것이 아니라 메모리에서 라우팅 처리를 한다.

\
MemoryRouter를 사용하는 이유는? : 테스트나 서버 측 렌더링과 같이 브라우저를 사용할 수 없는 환경에서 React.js 애플리케이션을 테스트하거나 렌더링할 때 사용한다.

BrowserRouter와 동일한 API를 가지고 있으며, Link 및 Route 구성 요소를 사용하여 React.js 애플리케이션에서 라우팅을 구현할 수 있다.
{% endhint %}

```jsx
describe('App', () => {
  function renderApp(path: string) {
    render(
      <MemoryRouter initialEntries={[path]}>
        {' '}
        // ✅ 현재 위치가 어디인지 모르기 때문에 initialEntries로 위치를 알려줘야
        한다.
        <App />
      </MemoryRouter>
    );
  }

  context('when the current path is “/”', () => {
    it('renders the home page', () => {
      renderApp('/');

      screen.getByText(/Hello/);
    });
  });

  context('when the current path is “/about”', () => {
    it('renders the about page', () => {
      renderApp('/about');

      screen.getByText(/About/);
    });
  });
});
```
