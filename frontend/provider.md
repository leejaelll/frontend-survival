---
description: props를 통해 데이터를 전달하는 방식 대신 다른 방법은 없을까?
cover: >-
  https://images.unsplash.com/photo-1715583198007-1ec0de4292bb?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTc0OTM3Nzl8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Provider 패턴

> _앱 내의 모든 컴포넌트들이 데이터에 접근해야 하는 경우 props로 작업하는 것은 매우 번거롭다. props를 하위 컴포넌트로 내리는 것은 코드가 지저분해지기 쉽상이다. 만약 data라는 프로퍼티의 이름을 변경해야하는 경우 모든 컴포넌트를 수정해야 한다._&#x20;

**데이터가 필요하지 않은 컴포넌트는 props를 받지 않도록 수정하는 것이 바람직하다. 👉🏻 **_**`Provider`**_** 패턴**



## Provider Pattern

#### 먼저 Provider로 감싼다.

```javascript
const DataContext = createContext();

function App() {
  const data = {...};
  
  return (
    <div>
      <DataContext.Provider value={data}>
        <Content />
      </DataContext.Provider>
    </div>
  )
}
```

* Provider 는 HOC로 `Context` 객체를 제공
* `createContexte` 메서드를 활용하면 Context 객체를 만들어낼 수 있다.&#x20;
* Provider 컴포넌트는 value라는 prop으로 하위 컴포넌트들에게 내려 줄 데이터를 받는다. 이 컴포넌트의 모든 자식 컴포넌트들은 해당 provider를 통해 value prop에 접근할 수 있다.&#x20;



#### 하위 컴포넌트들이 data에 접근하는 방법

```jsx
const { data } = React.useContext(DataContext);
```

* 각 컴포넌트는 `useContext` 훅을 활용하여 `data` 에 접근할 수 있다.&#x20;

***

### ThemeContext.Provider

```javascript
export const ThemeContext = createContext();

const themes = {
  light: {
    background: '#fff',
    color: '#000',
  },
  dark: {
    background: '#171717',
    color: '#fff',
  }
}

export default function App() {
  const [theme, setTheme] = useState('dark');
  
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light')
  }
  
  const providerValue = {
    theme: themes[theme],
    toggleTheme,
  }
  
  return (
    <div>
      <ThemeContext.Provider value={providerValue}>
        <Toggle />
        <Content />
      </ThemeContext.Provider> 
    </div>
  )
}
```

* Toggle 컴포넌트와 Content 컴포넌트가 ThemeContext Provider의 자식 컴포넌트로 존재하는 동안 value로 넘겼던 theme과 toggleTheme 값에 접근할 수 있다.&#x20;



### useContext

`Toggle` 컴포넌트 내에서는 테마 업데이트를 위해 `toggleTheme` 함수를 직접 호출할 수 있다.

```jsx
import React, { useContext } from 'react'
import { ThemeContext } from './App'

export default function Toggle() {
  const theme = useContext(ThemeContext)

  return (
    <label className="switch">
      <input type="checkbox" onClick={theme.toggleTheme} />
      <span className="slider round" />
    </label>
  )
}
```



***

## Hooks

각 컴포넌트에서 `useContext` 를 직접 import하는 대신 필요로 하는 컨텍스트를 직접 반환하는 훅을 구현할 수 있다.

```javascript
function useThemeContext() {
  const theme = useContext(ThemeContext)
  return theme
}
```



훅이 유효하게 사용되는지 검증하기 위해 컨텍스트가 falsy value일 때 예외를 발생시키도록 구현한다.

```jsx
function useThemeContext() {
  const theme = useContext(ThemeContext)
  if (!theme) {
    throw new Error('useThemeContext must be used within ThemeProvider')
  }
  return theme
}
```



컴포넌트들을 `ThemeContext.Provider` 로 직접 래핑하게 하는 것 대신. HOC를 만들어 간단하게 쓸 수 있도록 할 수 있다. 이렇게 하면 컨텍스트 로직과 렌더링 로직을 분리하여 재 사용성을 증가시킬 수 있다.

```jsx
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('dark')

  function toggleTheme() {
    setTheme(theme === 'light' ? 'dark' : 'light')
  }

  const providerValue = {
    theme: themes[theme],
    toggleTheme,
  }

  return (
    <ThemeContext.Provider value={providerValue}>
      {children}
    </ThemeContext.Provider>
  )
}

export default function App() {
  return (
    <div className={`App theme-${theme}`}>
      <ThemeProvider>
        <Toggle />
        <List />
      </ThemeProvider>
    </div>
  )
}
```

하위 컴포넌트들은 이제 `ThemeContext` 의 컨텍스트에 접근하기 위해 `useThemeContext` 훅을 사용하면 된다.

```jsx
export default function TextBox() {
  const theme = useThemeContext()

  return <li style={theme.theme}>...</li>
}
```

## 장점

* 컴포넌트 트리의 각 노드에 데이터를 전달하지 않아도 다수의 컴포넌트에 데이터를 전달할 수 있다.&#x20;
* prop-drilling을 하지 않아도 되기 때문에 앱의 데이터 흐름을 파악하기 편리하다.&#x20;
* 데이터가 필요없는 컴포넌트에 불필요하게 prop을 받을 필요가 없어진다.&#x20;

## 단점

* Provider 패턴을 과하게 사용할 경우, 특정상황에서 성능이슈가 발생할 수 있다. 컨텍스트를 참조하는 모든 컴포넌트는 컨텍스트가 변경될 때마다 모두 리렌더링 된다.&#x20;

