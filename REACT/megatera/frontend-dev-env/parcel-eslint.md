# Parcel & ESLint

## Parcel

**Zero Configuration**

* 특별한 설정 없이 바로 사용 가능한 빌드 도구. 내부적으로 SWC를 사용해 기존 도구보다 빠르다(ES Module을 적극 활용하는 Vite도 엄청나게 빠름).
* 참고:
  * [https://github.com/ahastudio/til/tree/main/parcel](https://github.com/ahastudio/til/tree/main/parcel)
  * [https://github.com/ahastudio/til/tree/main/vite](https://github.com/ahastudio/til/tree/main/vite)

### 필요한 설정 작업 두 가지

1.  `package.json` 파일에 source 속성 추가

    ```jsx
    "source": "./index.html",
    ```

    * source가 없을 때는 entry를 찾지 못하기 때문에 `npx parcel index.html —port 8080` 명령어를 입력해야 함
    * `"source": "./index.html"` 을 사용하면 index.html을 계속해서 쓰지 않아도 됨
2.  static 파일 처리

    * [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) 패키지 설치 후 `.parcelrc` 파일 작성.

    ```jsx
    npm i -D parcel-reporter-static-files-copy
    ```

    * 이렇게 하면 static 폴더의 파일을 정적 파일로 Serving할 수 있음

    ```jsx
    {
      "extends": ["@parcel/config-default"],
      "reporters":  ["...", "parcel-reporter-static-files-copy"]
    }
    ```

    * 실행시켜보면 `no such file or directory, stat ‘/Users/leejaelll/wor/my-app/static` 이라는 에러 메세지가 뜸 👉🏻 static 폴더가 없다는 이야기!
    * image와 같은 파일들은 모두 static 폴더 안에 있어야 함 (주의할 것은 경로에 '/static/'을 쓰지 않아야한다는 것!)

빌드 + 정적 서버 실행

```jsx
npx parcel build

npx servor ./dist
```

* [servor](https://github.com/lukejacksonn/servor)

\


## ESLint

🚀 [**ESLint 공식문서**](https://eslint.org/)

* [린트](https://ko.wikipedia.org/wiki/%EB%A6%B0%ED%8A%B8\_\(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4\))
* [정적 프로그램 분석](https://ko.wikipedia.org/wiki/%EC%A0%95%EC%A0%81\_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8\_%EB%B6%84%EC%84%9D)
* [Coding\_conventions](https://en.wikipedia.org/wiki/Coding\_conventions)

ESLint를 사용해야하는 이유가 있을까?

* 스타일 통일
* 잠재적 문제 발견
* 베스트 프랙티스 추천 → 최신 트렌드를 학습하는데 활용할 수 있다.

[VS Code ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

`.vscode/settings.json`파일을 추가해 JS/TS 파일을 저장할 때마다 ESLint를 실행하고 문제점을 고치게 설정할 수 있다.

```jsx
{
    "editor.rulers": [
        80
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "trailing-spaces.trimOnSave": true
}
```

아샬이 쓰는 VS Code 기본 세팅:

* [VS Code 기본 세팅](https://github.com/ahastudio/CodingLife/blob/main/20211008/react/.vscode/settings.json)
* [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)



## ✅ Keyword

### Bundler(번들러)

* 웹 개발에서 모듈 번들링을 담당하는 도구
* bundle의 사전적 의미는 묶음, 보따리를 의미한다.
* 모듈들을 하나의 파일로 묶어서 번들(Bundle)을 생성하는 것이 번들러의 주요 기능
* 대표적인 번들러로는 Webpack, Parcel 등이 있다.

### Parcel

* Parcel은 웹 애플리케이션을 빌드하기 위한 새로운 번들러 중 하나

🙋🏻‍♂️ Parcel의 특징은?

* 들링 시 기본적인 설정이 이미 되어 있으므로, 초기 설정 없이도 쉽게 프로젝트를 시작할 수 있다.
* 양한 모듈 시스템을 지원하고, 다양한 플러그인을 사용
* 코드의 수정이 있을 경우에는 자동으로 리로드가 되는 기능도 제공
* 하지만 Parcel은 빌드 시간이 오래 걸리는 문제가 있어 대규모 프로젝트에서는 다른 번들러와 비교하여 성능이 떨어질 수 있다.

### Lint(린트)

* 소스 코드에서 문제를 찾아내고 보고하는 도구
* 코드에서 발견된 문제란? 👉🏻 잠재적인 오류, 버그 또는 스타일 규칙 위반 등

🙋🏻 린트라는 용어는 언제 생긴걸까?

* 벨 연구소의 컴퓨터 과학자 스티픈 C. 존슨
* 1978년에 린트(lint)라는 용어를 창안

### ESLint

* ESLint는 코딩 컨벤션에 위배되는 코드나 안티 패턴을 자동 검출하는 도구

{% hint style="warning" %}
ESLint는 옳바른 코딩 습관을 갖도록 돕는 유용한 툴이다. 가급적이 아니라 필히 사용할 것을 권장한다.
{% endhint %}



### 정적 프로그램 분석 Vs. 동적 프로그램 분석

* 소프트웨어 분석의 두 가지 접근 방식

📚 **정적 프로그램 분석 (Static Program Analysis)**

* 소스 코드를 실행하기 전에 코드를 분석
* 소스 코드를 기반으로 코드의 일부분이나 전체를 분석하고 오류를 찾을 수 있다.
* 코드 내에서 **잠재적인 버그나 보안 취약점**을 찾기 위해 사용

📚 **동적 프로그램 분석 (Dynamic Program Analysis)**

* 실행 중인 프로그램을 분석하는 방법
* 소프트웨어를 실행하여 실행 중에 발생하는 문제를 찾는 데 사용
* 프로그램이 실행되는 동안 **데이터 값, 메모리 사용, 오류 및 예외 처리** 등을 분석
