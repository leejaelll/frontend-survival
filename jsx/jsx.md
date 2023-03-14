# JSX

## 📚 강의 정리

- "XML-like syntax extension to ECMAScript"
- JSX는 XML과 유사한 문법
- JSX에서 가장 중요한 것은? 문법 확장이라는 것

JSX는 React를 만들면서 나온 리액트의 부산물과 같다.하지만 JSX를 리액트에서만 사용하는 것은 아니다. Vue.js에서도 JSX를 사용할 수 있다.

{% hint style="info" %}

### JSX는 HTML이 아니다.

HTML과 매우 비슷하게 생겼지만, XML과 비슷한 특징을 가지고 있다.

XML은 보다 더 엄격한 문법을 가지고 있으며, 태그와 속성의 이름이나 값 등이 반드시 큰 따옴표로 둘러싸여야 한다. JSX에서도 이러한 특징이 나타난다.

예를 들면, <br> 태그를 HTML에서는 self-closing 태그가 없어도 사용가능하지만 XML에서는 `<br />` 처럼 작성해야 오류가 생기지 않는다.

{% endhint %}

JSX는 XML처럼 작성된 부분을 React.createElement을 쓰는 JavaScript 코드로 변환한다.
중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 **JavaScript 코드와 1:1로 매칭**된다.

JSX를 사용하는 방법

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

- “Presets”에서 “react”를 체크하거나, “Plugins”에서 “@babel/plugin-transform-react-jsx”를 추가하면 JSX를 실험할 수 있다.

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
  /*#__PURE__*/ React.createElement('p', null, 'Hello, world!');

  /*#__PURE__*/ React.createElement(
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
/*#__PURE__*/ React.createElement(
  React.Fragment,
  null,
  /*#__PURE__*/ React.createElement('p', null, 'Hello, world!'),
  /*#__PURE__*/ React.createElement(
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

---

## ✅ Keyword

- React에서 JSX를 사용하는 목적
- Syntactic sugar
- React.createElement
- React Element
- React StrictMode
- VDOM(Virtual DOM)이란?
  - DOM이란?
  - DOM과 Virtual DOM의 차이
- Reconciliation(재조정) 과정은 무엇인가?

---

## 🐋 Supplement

### XML Vs. XHTML

### [JSX로 마크업하는 방법](https://beta.reactjs.org/learn/writing-markup-with-jsx)

**JSX의 목적**

- 트리 구조를 정의하기 위한 간결하고 친숙한 구문을 정의하는 것
- 자바스크립트 코드로도 작성할 수 있지만, 트리구조를 직관적으로 볼 수 있기 때문에 사용한다고 생각하면 되겠다.
