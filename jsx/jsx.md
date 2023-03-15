# JSX

## 📚 강의 정리

## JSX

- "XML-like syntax extension to ECMAScript"
- JSX는 XML과 유사한 문법
- JSX에서 가장 중요한 것은? 문법 확장이라는 것

JSX는 React를 만들면서 나온 리액트의 부산물과 같다.하지만 JSX를 리액트에서만 사용하는 것은 아니다.  
Vue.js에서도 JSX를 사용할 수 있다.

{% hint style="info" %}

### JSX는 HTML이 아니다

HTML과 매우 비슷하게 생겼지만, XML과 비슷한 특징을 가지고 있다.

XML은 보다 더 엄격한 문법을 가지고 있으며, 태그와 속성의 이름이나 값 등이 반드시 큰 따옴표로 둘러싸여야 한다. JSX에서도 이러한 특징이 나타난다.

예를 들면, <br> 태그를 HTML에서는 self-closing 태그가 없어도 사용가능하지만 XML에서는 `<br />` 처럼 작성해야 오류가 생기지 않는다.

{% endhint %}

JSX는 XML처럼 작성된 부분을 React.createElement을 쓰는 JavaScript 코드로 변환한다.
중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 **JavaScript 코드와 1:1로 매칭**된다.

<br />

🚀 **JSX를 사용하는 방법**

```jsx
// Using JSX to express UI Components
var dropdown =
  <Dropdown>
    A dropdown list
    <Menu>
      <MeneItem>Do Something</MeneItem>
      <MeneItem>Do Something Fun</MeneItem>
      <MeneItem>Do Something Else</MeneItem>
    </Menu>

render(dropdown);
```

JSX코드를 JavaScript 코드로 변환하는 방법 👉🏻 변환기 중 제일 유명한 [Babel](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=Q&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.21.3&externalPlugins=&assumptions=%7B%7D)로 확인 가능.

> “Presets”에서 “react”를 체크하거나, “Plugins”에서 “@babel/plugin-transform-react-jsx”를 추가하면 JSX를 실험할 수 있다.

<br />

예제코드 보면서 JSX와 익숙해지기

### Example 1

```jsx
// JSX 코드
<p>Hello, world!</p>;

// 변환된 JS 코드
/*#__PURE__*/ React.createElement('p', null, 'Hello, world');
```

`/* @jsx */`를 코드 상단 주석에 작성하면 JSX를 JS로 변환할 때 React.createtElement가 아닌 작성한 이름대로 코드가 변환된다.

만약 리액트가 아닌 외부에서 JSX를 사용하고 싶다면 이 방식을 사용한다.

```jsx
/* @jsx Reeact.createElement */

<p>Hello, world</p>;

// 변환된 JS 코드
/* @jsx Reeact.createElement */

Reeact.createElement('p', null, 'Hello, world');
```

<br />

### Example 2

```jsx
// JSX 코드
<Greeting name="world" />;

// 변환된 JS 코드
/*#__PURE__*/ React.createElement(Greeting, {
  name: 'world',
});
```

- Greeting은 기본 태그가 아닌 사용자가 만든 컴포넌트
- 변환 코드에서도 첫 번째 인자에 Greeting 함수가 그대로 들어간다.

```jsx
function Greeting({ name }) {
  return <p>Hello, {name}</p>;
}

<Greeting name="world" />;

// JS 변환 코드
function Greeting({ name }) {
  return /*#__PURE__*/ React.createElement('p', null, 'Hello, ', name);
}
/*#__PURE__*/ React.createElement(Greeting, {
  name: 'world',
});
```

- `React.createElement(Greeting, {name: 'world',}`
  - 변환을 하지 않고 Greeting 함수를 그대로 넣어준 것을 볼 수 있다. 👉🏻 함수는 일급 객체이기 때문
- 두 번째 인자에는 `{name:'world'}` 👉🏻 속성들을 전달하는 자리
- 전달해야 할 속성이 2개 이상이라면 `{name: 'world', age: '13'}`
- 문자열이 아닌 숫자타입의 값을 주고 싶다면 `<Greeting name="world" age={13} />` 와 같이 전달한다.

<br />

### Example 3

```jsx
<Button type="submit">Send</Button>;

// 변환된 JS 코드
React.createElement(Button, { type: 'submit' }, 'Send');
```

onClick 이벤트를 추가하는 경우

```jsx
<Button onClick={onClickHandler} type="submit">
  Send
</Button>;

// 변환된 JS 코드
/*#__PURE__*/ React.createElement(
  Button,
  {
    onClick: onClickHandler,
    type: 'submit',
  },
  'Send'
);
```

- 속성에 대한 Object를 두 번째 인자에 넘겨준다.

<br />

### Exmaple 4

```jsx
<p>Hello, world!</p>
<Button type="submit">Send</Button>

// Error
/repl.jsx: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (2:0)
```

오류가 발생하는 이유

- 명시적으로 각각 따로 호출하면

  ```jsx
  React.createElement('p', null, 'Hello, world!');

  React.createElement(
    Button,
    {
      type: 'submit',
    },
    'Send'
  );
  ```

- 두 개를 묶어서 호출하게 되면 함수를 한 줄에 연속으로 두 개 호출한 것과 동일하다.

  ```jsx
  add(2, 4) add(3, 5);

  React.createElement('p', null, 'Hello, world!') React.createElement(Button, { type: 'submit', },'Send')
  ```

React.Fragment

```jsx
<React.Fragment>
  <p>Hello, world!</p>
  <Button type="submit">Send</Button>
</React.Fragment>;

// JS 변환 코드
React.createElement(
  React.Fragment,
  null,
  React.createElement('p', null, 'Hello, world!'),
  React.createElement(
    Button,
    {
      type: 'submit',
    },
    'Send'
  )
);
```

- `<React.Fragment>` 대신 `<></>`을 사용할 수도 있다.
- 변환된 JS 코드는 동일

```jsx
<div className="test">
  <p>Hello, world!</p>
  <Button type="submit">Send</Button>
</div>;

// 변환된 JS 코드
React.createElement(
  'div',
  { className: 'test' },
  React.createElement('p', null, 'Hello, world!'),
  React.createElement(Button, { type: 'submit' }, 'Send')
);
```

- 첫 번째 인자: 태그
- 두 번째 인자: 속성 값
- 세 번째 인자: `React.createElement('p', null, 'Hello, world!')`
- 네 번째 인자: `React.createElement(Button, { type: 'submit' }, 'Send')`

React Runtime 옵션을 사용하면 코드가 조금 달라진다.

```jsx
<React.Fragment>
  <p>Hello, world!</p>
  <Button type="submit">Send</Button>
</React.Fragment>;

// 변환된 JS 코드
import { jsx as _jsx } from 'react/jsx-runtime';
import { jsxs as _jsxs } from 'react/jsx-runtime';

/*#__PURE__*/ _jsxs(React.Fragment, {
  children: [
    /*#__PURE__*/ _jsx('p', {
      children: 'Hello, world!',
    }),
    /*#__PURE__*/ _jsx(Button, {
      type: 'submit',
      children: 'Send',
    }),
  ],
});
```

- 강의에서 본 것과는 코드가 살짝 다르다. (그 사이에 업데이트 된 것 같다.)
- react/jsx-runtime에서 가져온 `_jsx`를 사용한다. 👉🏻 이 코드가 요즘의 JSX

<br />

### Example 5

```jsx
<div>
  <p>Count: {count}!</p>
  <button type="button" onClick={() => setCount(count + 1)}>
    Increase
  </button>
</div>;

// 변환된 JS코드
React.createElement(
  'div',
  null,
  React.createElement('p', null, 'Count: ', count, '!'),
  React.createElement(
    'button',
    {
      type: 'button',
      onClick: () => setCount(count + 1),
    },
    'Increase'
  )
);
```

- jsx는 태그안에서 text와 JavaScript 값으로만 구분을 한다, 임의로 공간을 주려면 {' '}을 이용하면 createElement에 추가된다.

<br />

## React Element

- [JSX 없이 사용하는 React](https://ko.reactjs.org/docs/react-without-jsx.html)
- [createElement](https://beta.reactjs.org/reference/react/createElement)

```jsx
function Greeting({ name }) {
  return <p>Hello, {name}</p>;
}

// JSX 코드를 쓰지 않고 React.createElement를 써서 리액트를 사용해도 된다.

function Greeting({ name }) {
  return React.createElement('p', null, 'Hello, ', name);
}
```

그럼 JSX를 사용하는 이유가 뭘까?  
: React.createElement를 쉽게 사용하도록 하기 위해

{% hint style="info" %}

### JSX 대신 React.createElement를 써서 React Element 트리를 갱신하는데 쓸 수 있다.

`document.createElement('div')`를 하면 div element가 만들어지는 것과 동일하다.

React.createElement를 직접 쓰면 복잡하기 때문에 jsx-runtime에서는 \_jsx, Preact는 h란 함수를 지원한다.

{% endhint %}

<br />

## Virtaul DOM

- [Virtaul DOM](https://ko.reactjs.org/docs/faq-internals.html)
- [재조정 (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html) 👉🏻 [Reconciliation(재조정) 과정은 무엇인가? 내용 정리 🦖](#reconciliation재조정-과정은-무엇인가)

화면을 갱신하려면 DOM을 바꿔줘야한다.

```jsx
// DOM을 직접 조작한 경우
document.querySelector('.btn').textContent = '변경';
```

리액트는 바로 DOM을 조작하는 것이 아니라 화면 조작에 쓸 가상의 DOM 트리를 만들어서 사용한다. 👉🏻 [Virtual DOM과 Internals](#virtual-dom과-internals)

{% hint style="warning" %}

우리는 매번 작은 React Element 트리, VDOM 트리를 만든다. VDOM은 실제 DOM과 비교를 통해 변경사항을 적용한다.

{% endhint %}

## Virtual DOM을 쓰는 이유?

{% hint style="danger" %}

Virtual DOM를 사용하는 이유는 빠르기 때문이다.  
👉🏻 사실은 그렇지 않다!

{% endhint %}

가장 중요한 이유는 바로 아래 문장과 같다.

> 이 접근방식이 React의 선언적 API를 가능하게 합니다.

만약 신중하게 코드를 작성하고 어떤 데이터가 들어왔을 때 신중하게 DOM을 업데이트해야한다면 충분히 만들 수 있다. 하지만 리액트의 Virtual DOM을 사용한다면 신중하게 코드를 짜지 않아도 일단 넣으면 알아서 효율적인 렌더링을 해준다는 것.

애플리케이션을 만들 때 어디가 균형점일까?  
적절하게 빠르고 유지보수가 가능 vs 빠른 건 매우 빠름

👉🏻 리액트는 첫 번째를 선택(이게 바로 Virtual DOM)

- VDOM이 무엇이고, 왜 쓰는지 안다면 활용할 수 있는 [최적화 기법](https://ko.reactjs.org/docs/optimizing-performance.html)이 존재함.

---

## ✅ Keyword

### React에서 JSX를 사용하는 목적

- 트리 구조를 정의하기 위한 간결하고 친숙한 구문을 정의하는 것
- 자바스크립트 코드로도 작성할 수 있지만, 트리구조를 직관적으로 볼 수 있기 때문에 사용한다고 생각하면 된다.

<br />

- Syntactic sugar
- React.createElement
- React Element
- React StrictMode
- VDOM(Virtual DOM)이란?
  - DOM이란?
  - DOM과 Virtual DOM의 차이

### Reconciliation(재조정) 과정은 무엇인가?

---

## 🐋 Supplement

### XML Vs. XHTML

### DOM Element Vs. React Element

### JSX로 마크업하는 방법

### Virtual DOM과 Internals
