# Router

## 학습 키워드

* ReactRouter - RouterProvider



### 🫥 라우터 객체를 만들어서 사용하는 방법

라우팅 정보만 별도로 분리해서 관리할 수 있다.

```jsx
export default function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/about" element={<AboutPage />} />
        </Routes>
      </main>
      <Footer>
    </div>
  );
}
```

👉🏻 위 코드는 페이지의 구조를 나타내는 레이아웃과 라우팅처리가 같이 작성되어 있다.

\


### 레이아웃 구조와 라우팅 처리를 분리하자!

라우터와 레이아웃을 분리하면 다음과 같다.

```jsx
<div>
  <Header />
  <main>
    <Page />
  </main>
  <Footer />
</div>
```

```jsx
function Page() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />}>
      <Route path="/about" element={<AboutPage />}>
    </Routes>
  )
}
```

👉🏻 객체 형태로 만들 순 없을까?

```jsx
const routes = [
  {
    element: <Layout />,
    childern: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> }, // ✅ childern안에 있는 것들은 전부 Layout으로 그려준다.
    ],
  },
];

const router = createBrowserRouter(routes); // ✅ App 컴포넌트를 거치지 않고 바로 브라우저 라우터 만들어서 사용할 수 있도록 한다.

root.render(
  <React.StrictMode>
    <RouterProvider router={router} /> // ✅ props으로 router 전달하기
  </React.StrictMode>
);
```

`<Layout>` 컴포넌트는 어떻게 생겨야할까?

```jsx
import { Outlet } from 'react-router-dom';

export default function Layout() {
  return (
    <div>
      <Header />
      <Oulet /> // 🚧 URL에 맞는 컴포넌트를 렌더링시켜주는 역할을 한다.
      <Footer>
    </div>
  )
}
```

{% hint style="info" %}
#### 리액트에서 라우터를 분리하는 이유가 뭘까?

: 코드의 구성과 관심사의 분리가 잘 이루어진다. 라우터는 네비게이션 로직을 처리하여 현재 URL에 기반하여 어떤 컴포넌트를 렌더링해야하는지 결정한다. 이러한 로직을 모듈이나 파일로 분리하면 다른 부분에 영향을 주지 않고 라우팅 코드를 유지하고 업데이트하기가 더 쉬워진다.
{% endhint %}

\


### 🫥 각 파일을 모듈로 분리했을 때 코드를 한 눈에 보기

```jsx
// App.tsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import routes from './routes';

const router = createBrowserRouter(routes);

export default function App() {
  return (
    <RouterProvider router={router}>

  )
}
```

```jsx
// routes.tsx
const routes = [
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> },
    ],
  },
];

export default routes;
```

```jsx
// Layout.tsx
import { Outlet } from 'react-router-dom';

export default function Layout() {
  return (
    <div>
      <Header />
      <Oulet />
      <Footer>
    </div>
  )
}
```

\


라우터 객체를 만드는 방식으로 변경하면 테스트 코드에도 변경이 생긴다.

중복을 제거하기 위해서 테스트마다 path만 변경되기 때문에, path를 매개변수로 받는 renderRouter 함수를 하나 작성해서 사용한다.

```jsx
describe('routes'', () => {
 function renderRouter(path: string) {
  const router = createMemoryRouter(routes, { initialEntries: [path] });
  render(<RouterProvider router={router} />);
 }

 context('when the current path is “/”', () => {
  it('renders the home page', () => {
   renderRouter('/');

   screen.getByText(/Hello/);
  });
 });

 context('when the current path is “/about”', () => {
  it('renders the about page', () => {
   renderRouter('/about');

   screen.getByText(/About/);
  });
 });
});
```
