# Global Style & Theme

## 학습 키워드

* Reset CSS
* `box-sizing` 속성
* `word-break` 속성
* Theme
* ThemeProvider

\


styled-reset을 설치하여 모든 속성값을 초기화시킬 수 있다.

```JSX
npm i styled-reset
```

\


## 전역 스타일 설정하기

글로벌 스타일을 지정하는 것은 styled-components에 포함되어 있고 createGlobalStyle을 통해 만들 수 있다.

```JSX
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  button {
    padding : 5px;
    margin : 2px;
    border-radius : 5px;
  }
`

function App() {
  return (
    <>
      <GlobalStyle />
      <Button>전역 스타일 적용하기</Button>
    </>
  );
}
```

### Global Style

> [createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)

> [대체 CSS box model](https://developer.mozilla.org/ko/docs/Learn/CSS/Building\_blocks/The\_box\_model#%EB%8C%80%EC%B2%B4\_css\_box\_model)

> [box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)

> [The 62.5% Font Size Trick](https://www.aleksandrhovhannisyan.com/blog/62-5-percent-font-size-trick/)

> [keep-all-villain](https://twitter.com/keepallvillain)

> [word-break](https://developer.mozilla.org/ko/docs/Web/CSS/word-break)

\
