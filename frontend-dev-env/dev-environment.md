## 📚 강의 정리

Node.js를 기반으로 자바스크립트 환경 세팅하는 방법에 대해서 학습해보자.
Deno를 사용한다면 개발 환경 세팅이 간단하겠지만, 대부분 Node.js를 기반으로 하고 있기 때문에 기본적으로 Node.js를 알아야 한다.
'본질의 중요성'이라는 관점에서 보면 리액트도 비슷한 것 같다. 리액트를 쉽고 편하게 사용하려면 자바스크립트를 깊게 이해하고 있어야 한다는 것.

### JavaScript 개발 환경 (Node.js) 세팅

첫 번째로 해야할 일은 Node.js를 설치하고, 프로젝트를 진행할 수 있도록 Node.js 패키지를 만들어야 한다.
프로젝트를 진행할 때마다 항상 같은 버전을 쓸 수 없는 상황이 생긴다.

예를 들면 프로젝트 때 사용할 외부 패키지가 해당 버전을 지원하지 않는 경우가 있다. 그래서 여러 버전을 설치해야하는 경우들이 생기는데 이때 `fnm` 을 사용하면 편리하다.

```jsx
// install fnm
brew install fnm

// ~/.zshrc에 환경변수 추가하기
eval "$(fnm env)"
```

> iterm에서 작업을 진행했는데, `~/.zshrc`에 코드를 입력하고 난 후 프로그램을 종료했다가 다시 실행해야 해당 변수를 인식한다. **_(fnm env를 찾을 수 없다는 오류때문에 해결하기 위해서 공식문서에 있는 해결책도 써봣지만 아주 간단히 해결할 수 있는 문제였다..🥲)_**

LTS(Long Term Support) 버전을 설치하자.

```jsx
// Node.js LTS 버전 설치
fnm intall --lts

// 설치된 리스트 확인하기
fnm list

// 지정한 버전 default로 설정하기
fnm default $(fnm current)

// 현재 기준 LTS 최신버전 확인하기
node -v
```

### TypeScript + React + Jest + ESLint + Parcel 개발 환경 세팅

프로젝트에 필요한 환경들을 세팅해야할 때 어떤 순서로 진행하면 좋을까?

순서로 기억해서 습관을 들여보자 👏🏻

1. 프로젝트 폴더 생성
2. npm init -y
3. .gitignore 파일 생성
4. 타입스크립트 설치
5. ESLint 설치
6. 리액트 설치
7. 테스팅 도구(Jest) 설치
8. Parcel 설치

하나씩 자세히 살펴보면 다음과 같다.

1. 프로젝트 폴더 생성

   ```jsx
   // my-project 라는 폴더 생성
   mkdir my-project
   // 만든 폴더로 이동
   cd my-project
   // VSCode로 해당 폴더 열기
   code .
   ```

2. npm init -y

   - npm init 명령어는 Node.js 프로젝트를 시작할 때 사용하는 명령어
   - 프로젝트의 이름, 버전, 설명, 기술과 같은 정보를 입력한다.
   - 이 정보를 입력하면 `package.json` 파일을 생성하고 내가 입력한 정보들이 이 곳에 저장된다.
   - `npm init -y` 명령어를 실행하면 정보를 입력하지 않아도 package.json이 생성된다. 이후 package.json에 정보를 수정할 수 있다.

3. `.gitignore` 파일 생성

-

## ✅ Keyword

### Node.js

- Node.js
- NPM(Node Package Manager)
  - package.json / package-lock.json
  - node_modules
  - npx
- ES Modules vs CommonJS

## 🐋 Supplement

### fnm(Fast Node Manager) Vs. nvm(Node Version Manager)

둘 다 노드 버전을 관리하는데 사용한다.
