# styled-components

## 학습 키워드

- styled-componets

<br />

{% hint="info"%}
유명한 라이브러리들은 대부분 ‘왜’ 만들어졌는지 혹은 품고 있는 철학 등이 잘 정리되어 있는 경우가 많다.. 라이브러리를 사용할 때는 공식문서를 꼭 살펴보는 습관을 가지기

참고 레퍼런스: https://styled-components.com/docs/basics

{% endhint%}

<br />

## 🫥 Styled Components를 쓰게 된 이유는?

개발자들 CSS코드를 다룰 때 느꼈던 불편한 점이 무엇이었을까?

- class, id 이름을 짓느라 고민한 적이 있다.
- CSS 파일 안에서 내가 원하는 부분을 찾기 힘들었다.
- CSS 파일이 너무 길어져서 파일을 쪼개서 관리해 본 적이 있다.
- 스타일 속성이 겹쳐서 내가 원하는 결과가 나오지 않은 적이 있다.

👉🏻 이런 불편함을 CSS를 컴포넌트화 시킴으로써 해결해 주는 라이브러리가 바로 Styled Components

![](https://s3.ap-northeast-2.amazonaws.com/urclass-images/j-ZVe6U1zFdkK2Jex_JFl-1655885520482.png)

> Styled Components는 앞서 배운 CSS in JS라는 개념이 대두되면서 나온 라이브러리

CSS in JS 라이브러리를 사용하면 CSS도 쉽게 Javascript 안에 넣어줄 수 있으므로, HTML + JS + CSS까지 묶어서 하나의 JS파일 안에서 컴포넌트 단위로 개발할 수 있게된다.

## Styled Components 설치하기

Babel Plugin을 SWC에서 쓸 수 있도록 포팅한 것도 함께 설치하자(SSR 지원 등을 위한 공식 권장사항).

```JSX
npm i styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

.swcrc 파일 작성

```JSX
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```
