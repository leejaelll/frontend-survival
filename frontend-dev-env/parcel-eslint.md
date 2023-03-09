## 📚 강의 정리

## Parcel

**Zero Configuration**

- 특별한 설정 없이 바로 사용 가능한 빌드 도구. 내부적으로 SWC를 사용해 기존 도구보다 빠르다(ES Module을 적극 활용하는 Vite도 엄청나게 빠름).
- 참고:
  - [https://github.com/ahastudio/til/tree/main/parcel](https://github.com/ahastudio/til/tree/main/parcel)
  - [https://github.com/ahastudio/til/tree/main/vite](https://github.com/ahastudio/til/tree/main/vite)

<br>

### 필요한 설정 작업 두 가지

1. `package.json` 파일에 source 속성 추가

   ```jsx
   "source": "./index.html",
   ```

   - source가 없을 때는 entry를 찾지 못하기 때문에 `npx parcel index.html —port 8080` 명령어를 입력해야 함
   - `"source": "./index.html"` 을 사용하면 index.html을 계속해서 쓰지 않아도 됨

2. static 파일 처리

   - [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) 패키지 설치 후 `.parcelrc` 파일 작성.

   ```jsx
   npm i -D parcel-reporter-static-files-copy
   ```

   - 이렇게 하면 static 폴더의 파일을 정적 파일로 Serving할 수 있음

   ```jsx
   {
     "extends": ["@parcel/config-default"],
     "reporters":  ["...", "parcel-reporter-static-files-copy"]
   }
   ```

   - 실행시켜보면 `no such file or directory, stat ‘/Users/leejaelll/wor/my-app/static` 이라는 에러 메세지가 뜸 👉🏻 static 폴더가 없다는 이야기!
   - image와 같은 파일들은 모두 static 폴더 안에 있어야 함

빌드 + 정적 서버 실행

```jsx
npx parcel build

npx servor ./dist
```

- [servor](https://github.com/lukejacksonn/servor)

<br>

## ESLint

🚀 [**ESLint 공식문서**](https://eslint.org/)

- [린트](<https://ko.wikipedia.org/wiki/린트_(소프트웨어)>)
- [정적 프로그램 분석](https://ko.wikipedia.org/wiki/정적_프로그램_분석)
- [Coding_conventions](https://en.wikipedia.org/wiki/Coding_conventions)

ESLint를 사용해야하는 이유가 있을까?

- 스타일 통일
- 잠재적 문제 발견
- 베스트 프랙티스 추천 → 최신 트렌드를 학습하는데 활용할 수 있다.

[VS Code ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

`.vscode/settings.json`파일을 추가해 JS/TS 파일을 저장할 때마다 ESLint를 실행하고 문제점을 고치게 설정할 수 있다.

```jsx
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

아샬이 쓰는 VS Code 기본 세팅:

- [VS Code 기본 세팅](https://github.com/ahastudio/CodingLife/blob/main/20211008/react/.vscode/settings.json)
- [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)

---

## ✅ Keyword

### Bundler(번들러)

### Parcel

### Lint(린트)

### ESLint

---

## 🐋 Supplement

### 정적 프로그램 분석 Vs. 동적 프로그램 분석
