## 📚 강의 정리

Node.js를 기반으로 자바스크립트 환경 세팅하는 방법에 대해서 학습해보자.
Deno를 사용한다면 개발 환경 세팅이 간단하겠지만, 대부분 Node.js를 기반으로 하고 있기 때문에 기본적으로 Node.js를 알아야 한다.
'본질의 중요성'이라는 관점에서 보면 리액트도 비슷한 것 같다. 리액트를 쉽고 편하게 사용하려면 자바스크립트를 깊게 이해하고 있어야 한다는 것.

## JavaScript 개발 환경 (Node.js) 세팅

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

<br>

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

<br>

## TypeScript + React + Jest + ESLint + Parcel 개발 환경 세팅

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

<br>

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

   <br>

2. npm init -y

   - npm init 명령어는 Node.js 프로젝트를 시작할 때 사용하는 명령어
   - 프로젝트의 이름, 버전, 설명, 기술과 같은 정보를 입력한다.
   - 이 정보를 입력하면 `package.json` 파일을 생성하고 내가 입력한 정보들이 이 곳에 저장된다.
   - `npm init -y` 명령어를 실행하면 정보를 입력하지 않아도 package.json이 생성된다. 이후 package.json에 정보를 수정할 수 있다.

<br>

3. `.gitignore` 파일 생성

   - .gitignore 파일은 필수
   - 실행되는 모든 패키지가 저장되어있는 node_modules 폴더는 매우매우 무겁고 올릴 필요가 없다.
   - .gitignore는 Git에게 '여기 써져있는 폴더나 파일은 신경쓰지 말아줘'라는 전달해주는 역할을 한다.
   - .gitignore 파일을 쉽게 만들 수 있는 사이트(https://www.toptal.com/developers/gitignore/)

<br>

4. 타입스크립트 설치

   ```jsx
   npm i -D typescript
   ```

   - -D가 붙는 이유는? 👉🏻 타입스크립트는 개발자가 사용하기 위한 도구이지, 배포할 때는 필요가 없기 때문이다.
   - `--save-dev`를 줄여서 `-D`라고 쓸 수 있다. 이 옵션으로 설치된 패키지는 devDependecies에 들어간다.
     ![](./images/2023-03-07-15-09-43.png)

   ```jsx
   npx tsc --init
   ```

   - tsc는 타입스크립트 컴파일러
   - npm으로 설치를 하고나면 node_modules에 .bin 폴더에 실행 파일이 생겨있는걸 볼 수 있다.
   - 실행시키려면? 👉🏻 `./node_modules/.bin/tsc` 로 실행 시킬 수 있다.
     - 그럼 npx 명령어를 쓰는 이유는 뭘까?
     - 첫 번째로 써야하는 코드의 길이가 줄어든다. `./node_modules/.bin/`를 모두 지우고 `npx tsc`만 입력했을 때 동일하게 동작한다.
     - 두 번째 이유로는 로컬에 설치하지 않고도 프로그램을 실행시킬 수 있는 장점이 있다.
   - 명령어를 실행시키고 나면 tsconfig.json 파일이 만들어진다.
   - `tsconfig.json` 파일에서 한 가지 수정하기
     - `"jsx": "react-jsx"`

<br>

5. ESLint 설치

   ```jsx
   npm i -D eslint

   // typescript와 마찬가지로 설정파일을 만들어줘야함
   npx eslint --init
   ```

   - 설정을 모두 끝내면 .eslintrc.js 파일이 생성됨
   - `.eslintrc.js` 설정 수정하기
     - jest를 쓸거니까 `jest: true`미리 설정해놓기
   - **.eslintignore 파일 잊지말고 만들기**

<br>

6. 리액트 설치

   ```jsx
   npm i react react-dom

   // 타입스크립트를 사용하기 위한 패키지 설치
   // 타입스크립트와 동일하게 타입에 대한 부분은 배포될 때 필요한 것들이 아니기 때문 -D 옵션을 사용해 설치한다.
   npm i -D @types/react @types/react-dom
   ```

<br>

7. 테스팅도구 설치

   ```jsx
   npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
   ```

   - jest는 들어본 적이 있는데, SWC가 뭐지? 👉🏻 [SWC](#swc)
   - jest가 기본적으로 타입스크립트와 SWC를 사용하지 않기 때문에 추가적으로 설정이 필요하다.
     - jest.config.js 파일 생성
     - 참고(https://github.com/ahastudio/CodingLife/blob/main/20220726/react/jest.config.js)

<br>

8. Parcel 설치
   - parcel을 사용해서 데브 서버를 띄울 수 있다.
   - index.html을 하나 만들어서 텍스트를 입력한 후 `npm run start`를 하면? 👉🏻 `Could not find entry:`라는 오류 메세지를 보여준다.
     - Node 같은 경우 실행할 때 package.json 이라는 파일에 main에 있는 파일을 entry point로 잡는다.
     - 하지만 우리는 웹 서버를 띄울 것이기 때문에 `"source": "./index.html"`를 적어주어야 한다.

---

## ✅ Keyword

### Node.js

### NPM(Node Package Manager)

#### package.json / pacakge-lock.json

#### node_modules

#### npx

### ES Modules Vs. Common JS

---

## 🐋 Supplement

### Deno

### fnm(Fast Node Manager) Vs. nvm(Node Version Manager)

둘 다 노드 버전을 관리하는데 사용한다.

### dependencies Vs. devDependecies

### SWC
