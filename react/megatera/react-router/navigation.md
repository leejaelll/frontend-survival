# Navigation

## 학습 키워드

* Web APIs - History
* React Router - NavLink, Link, Navigate, useNavigate



### 🫥 페이지가 이동할때마다 계속 렌더링되는데... 변경되는 부분만 렌더링시킬 순 없나?

_HTML5부터는 `window.history.pushState`를 제공한다._

```jsx
const handleClick = (event: SyntheticEvent) => {
  const state = {};
  const title = '';
  const url = '/about';

  event.preventDefault();
  window.history.pushState(state, title, url);
};
```

## Link

리액트에서는 Link로 pushState와 같은 기능을 제공한다.

```jsx
<Link to="/">Home</Link>
```

👉🏻 Link는 pushState와 동일하게 리렌더링을 하지 않는다.

\


## NavLink

Link와 동일하게 해당 URL로 이동한다.

```jsx
<NavLink to="/">Home</NavLink>
```

Link와 차이점이 있다면?\
: 현재 위치하는 URL의 a 태그에 active 클래스를 자동으로 붙여준다.

![NavLink](<images/스크린샷 9.png>)

\


## Navigate

무조건 redirection 해주는 역할을 한다.

\


**로그아웃 버튼을 누르면 메인페이지로 다시 돌아가야한다고 가정해보자.**

로그아웃 버튼을 누르면 '/logout' URL로 이동하는 것이 아니라 '/'로 이동해야 한다. 👉🏻 이럴 때 사용하는 것이 `Navigate`

```jsx
export default function Logout() {
  return (
    <Navigate to "/" />
  )
}
```

> 테스트에서 “ReferenceError: Request is not defined” 에러가 나면 “whatwg-fetch”를 임포트해서 해결할 수 있다.

## useNavigate

Hook으로도 Navigate를 얻을 수 있다.

```jsx
import { useNavigate } from 'react-router-dom';

export default function Footer() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/about');
  };

  return (
    <button type="button" onClick={handleClick}>
  )
}
```
