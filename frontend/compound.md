# Compound 패턴

> 앱을 개발하다 보면 서로를 참조하는 컴포넌트를 만들기도 한다. 컴포넌트들은 서로 상태를 공유하기도 하고 특정 로직을 함께 사용하기도 한다. 컴파운드 컴포넌트 패턴은 여러 컴포넌트들이 모여 하나의 동작을 할 수 있게 해준다.&#x20;



## Compound 패턴

_<mark style="color:red;">**하나의 작업을 위해 여러 컴포넌트를 만들어 역할을 분담하게 한다.**</mark>_&#x20;



사진 목록 보여줄 때 각각의 사진을 수정하거나 삭제하도록 하려고 한다. Flyout 컴포넌트를 구현하여 사용자가 메뉴를 누르면 토글할 수 있도록 할 수 있다.&#x20;



#### Flyout 컴포넌트에 구현해야 할 요소

* 토글 버튼과 메뉴 리스트를 포함하는 Flyout Wrapper
* 메뉴를 토글할 수 있는 Toggle 버튼
* 메뉴를 포함한 List 컴포넌트



### Flyout 컴포넌트 구현

```jsx
const FlyOutContext = createContext()

function FlyOut(props) {
  const [open, toggle] = useState(false)

  const providerValue = { open, toggle }

  return (
    <FlyOutContext.Provider value={providerValue}>
      {props.children}
    </FlyOutContext.Provider>
  )
}
```

* state
  * 메뉴가 열렸는지 닫혔는지 여부를 나타내는 open
  * 토글 가능한 toggle 함수
* 자식 컴포넌트들이 받을 수 있도록 `FlyOutProvider` 컴포넌트에 전달

### Toggle 컴포넌트 구현

```jsx
function Toggle() {
  const { open, toggle } = useContext(FlyOutContext)

  return (
    <div onClick={() => toggle(!open)}>
      <Icon />
    </div>
  )
}
```

* 사용자가 토글 버튼을 눌렀을 때 나타날 메뉴를 렌더링

```javascript
const FlyOutContext = createContext()

function FlyOut(props) {
  const [open, toggle] = useState(false)

  return (
    <FlyOutContext.Provider value={{ open, toggle }}>
      {props.children}
    </FlyOutContext.Provider>
  )
}

function Toggle() {
  const { open, toggle } = useContext(FlyOutContext)

  return (
    <div onClick={() => toggle(!open)}>
      <Icon />
    </div>
  )
}

FlyOut.Toggle = Toggle
```

* `Toggle` 컴포넌트가 `FlyOutContext` 프로바이더에 접근할 수 있도록 해당 컴포넌트는 `FlyOut` 의 자식 컴포넌트로 렌더링해야 한다.
* 따라서 단순히 자식 컴포넌트로 렌더링 하면 되지만. 여기서는 `FlyOut` 컴포넌트의 Static property로 만들고 있다.



### Menu 컴포넌트 구현

open이 true일 때, 메뉴가 화면에 나와야 한다. 메뉴는 List와 Item 컴포넌트로 구현한다.&#x20;

```jsx
function List({ children }) {
  const { open } = React.useContext(FlyOutContext)
  return open && <ul>{children}</ul>
}

function Item({ children }) {
  return <li>{children}</li>
}

FlyOut.List = List
FlyOut.Item = Item
```



지금까지 구현한 것들 모두 `FlyOut` 컴포넌트만 가지고 사용할 수 있다.&#x20;

```jsx
import React from 'react'
import { FlyOut } from './FlyOut'

export default function FlyoutMenu() {
  return (
    <FlyOut>
      <FlyOut.Toggle />
      <FlyOut.List>
        <FlyOut.Item>Edit</FlyOut.Item>
        <FlyOut.Item>Delete</FlyOut.Item>
      </FlyOut.List>
    </FlyOut>
  )
}
```

* `FlyOutMenu` 자체에는 아무런 상태를 가지고 있지 않다.

## 장점

* 컴파운드 패턴은 동작 구현에 필요한 상태를 내부적으로 가지고 있는데 이것을 사용하는 쪽에서는 드러나지 않아 걱정 없이 사용할 수 있다.
* 자식 컴포넌트들을 일일히 import할 필요 없이 기능을 이용할 수 있다.

## 단점

* 내부에서 `React.Children.map`을 사용하고 있기 때문에 쓰는 쪽에서 자식 컴포넌트를 약속된 형태로 넘겨야 하는 제약이 생긴다.
* 엘리먼트를 복제하는 경우. 복제 대상 컴포넌트가 기존에 갖고 있는 prop과 이름이 충돌될 수 있다.

***

<details>

<summary>code</summary>

```typescript
function Page() {
  return(
    <div>
      <ImageList />
    <div>
  )
}
```

{% code title="ImageList.js" %}
```typescript
export default function ImageList() {
  const sources = [
    "https://images.pexels.com/photos/939478/pexels-photo-939478.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
    "https://images.pexels.com/photos/1692984/pexels-photo-1692984.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
    "https://images.pexels.com/photos/162829/squirrel-sciurus-vulgaris-major-mammal-mindfulness-162829.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  ];
  
  return sources.map((source, i) => <Image source={source} key={i} />); 
}

function Image({ source }) {
  return (
    <div>
      <imag src={source} />
      <FlyOutMenu />
    </div>
  )
}
```
{% endcode %}

{% code title="FlyOutMenu.js" %}
```javascript
export default function FlyOutMenu() {
  return (
    <FlyOut>
      <FlyOut.Toggle />
      <FlyOut.List>
        <FlyOut.Item>Edit</FlyOut.Item>
        <FlyOut.Item>Delete</FlyOut.Item>
      </FlyOut.List>
    </FlyOut>
  )
}
```
{% endcode %}

{% code title="FlyOut.js" %}
```javascript
const FlyOutContext = createContext();

export function FlyOut(props){
  const [open, toggle] = useState(false);
  
  return (
    <div>
      <FlyOutContext.Provider value={{ open, toggle }}>
        {props.children}
      </FlyOutContext.Provider>
    </div>
  );
}

const Toggle = () => {
  const { open, toggle } = useContext(FlyOutContext);
  
  return (
    <div onClick={() => toggle(!open)}> 
      <Icon />
    </div>
  );
}

const List = ({ children }) => {
  const { open ] = useContext(FlyOutContext);
  return open && <ul className="flyout-list">{children}</ul>;
}

const Item =({ children }) => {
  return <li>{children}</li>
}


FlyOut.Toggle = Toggle;
FlyOut.List = List;
FlyOut.Item = Item;

```
{% endcode %}



</details>
