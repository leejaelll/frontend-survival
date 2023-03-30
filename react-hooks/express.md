## 학습 키워드

- Express 란 👉🏻 [Express 실습해보기](https://leejaelll.github.io/2022/221112-archive/?highlight=express)
- URL 구조 👉🏻 [URL vs URI](https://leejaelll.github.io/2023/230330-2-archive/)
- REST API 👉🏻 [REST API란 무엇인가](https://leejaelll.github.io/2023/230329-archive/)
- HTTP Method(CRUD)

---

## Express

- [Express](https://expressjs.com/ko/)

Node.js 초창기때부터 존재했던 서버 웹 프레임워크

백엔드 이해 없이 프론트만 작업하는 것은 불가능. 백엔드를 어느정도는 이해하고 있어야 함

### 간단한 서버 앱 npm 패키지 세팅

- [Express 설치](https://expressjs.com/ko/starter/installing.html)

- [ts-node](https://github.com/TypeStrong/ts-node)

```jsx
mkdir express-demo-app

cd mkdir exrpress-demo-app

npm init -y

```

node_modules 파일이 올라가는 일이 없게 가장 먼저 .gitignore 파일 만들기

```jsx
touch .gitignore
echo "node_modules" > .gitignore
```

타입스크립트 설치

```jsx
npm i -D typescript

npx tsc --init
```

ts-node 설치

```jsx
npm i -D ts-node
```

ts-node 실행 명령어 👉🏻 npx ts-node

{% hint style="info" %}

원래 타입스크립트를 node로 실행하려면 타입스크립트를 자바스크립트로 컴파일하는 작업이 필요한데 ts-node를 사용하면 바로 실행이 가능

{% endhint %}

eslint 설치

```jsx
npm i -D eslint

npx eslint --init
```

"What type of modules does your project use?" 질문에 무엇을 골라야할까?

- 기본적으로 Node.js는 CommonJS 구현체
- 타입스크립트를 쓰면 import/export를 사용할 수 있기 때문에 JavaScript module을 사용

<br />

🦖 6:40 - 왜 타입스크립트를 쓰면 JavaScript module을 사용할 수 있을까?

- TypeScript는 ESModule, CommonJS를 모두 지원하기 때문에 Node.js 환경에서 JavaScript Module을 사용할 수 있다.

<br />

Express 설치

```jsx
npm i express

npm i -D @types/express
```

<br />

### Hello World 예제

- [Express 예제](https://expressjs.com/ko/starter/hello-world.html)

TypeScript에 맞춰서 app.ts 파일 작성

```jsx
import express from 'express';

const port = 3000;

// express를 호출하면 app이라는 인스턴스가 만들어짐
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, world!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

<br />

🦖 10:44 - listen은 정확히 무슨 역할을 하는 메서드일까?

- 지정된 포트에서 연결을 바인드하고 수신을 대기하는 메서드
- 지정된 포트와 호스트에서 들어오는 요청하는 새 HTTP 서버를 생성하고 요청을 수신되면 적절한 미들웨어 기능을 호출하여 요청을 처리하고 응답을 생성한다.
- listen 메서드는 두 가지 인수를 받는데 첫 번째 인수는 서버가 들어오는 요청을 수신해야하는 포트 번호
- 두 번째 인수는 서버가 성공적으로 시작되면 동작시킬 콜백 함수

ts-node로 실행

```jsx
npx ts-node app.ts

// 코드를 간소화하고싶다면 package.json에 script 추가하기
"start": "npx ts-node app.ts"
```

메세지를 바꿀 수도 있음

```jsx
app.get('/', (req, res) => {
  res.send('Hello, world!!!!');
});
```

👉🏻 새로고침해도 바뀌지 않음.

<br />

🦖 12:34 - 브라우저를 새로고침해도 수정된 내용이 반영되지 않는 이유가 뭘까?

- 응답이 한 번 클라이언트에 전달되면 res.send() 메서드 내부 내용에 대한 변경사항은 페이지 새로고침 후에도 클라이언트 측에 반영되지 않는다.

- 응답은 이미 클라이언트에 전송되었고 클라이언트는 서버에서 받은 콘텐츠를 기반으로 페이지를 렌더링 완료했기 때문

- 페이지를 새로고침하면 클라이언트는 동일한 리소스에 대한 새 요청을 보냄

- res.send() 메서드 내부의 동일한 내용과 함께 이전과 동일한 응답을 다시 보냄

- 클라이언트 측에 표시되는 콘텐츠를 업데이트하려면 서버 측에서 응답을 생성하는 코드를 수정하고 변경 사항을 저장한 다음 서버를 다시 시작해야 한다.

<br />

코드를 수정할 때마다 서버를 재실행해야 하는 문제를 피하기 위해 [nodemon](https://github.com/remy/nodemon) 사용.

```jsx
npm i -D nodemon

npx nodemon app.ts

// script 변경
"start": "npx nodemon app.ts"
```

<br />

## REST API

- Roy Fielding - “[Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)” (2000)

- [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)

대개는 “필딩 제약 조건” 4가지를 모두 만족하지 않고, Resource와 HTTP Verb만 도입하는 수준으로 사용.

- 이렇게 안 하고: /write-post

- 이렇게 하자: /posts → 뭔가를 한다 (CRUD)

<br />

🦖 17:24 - REST API에서 말하는 필딩 제약 조건이란?

- 클라이언트-서버 아키텍쳐: 시스템은 클라이언트와 서버가 분리되어 독립적으로 발전할 수 있게 설계되어야 한다.

- 상태 비저장: 클라이언트에서 서버로의 각 요청에는 요청을 완료하는데 필요한 모든 정보가 포함되어야 한다.

- 캐시 가능성: 서버의 응답은 캐시 가능 또는 캐시 불가능으로 명시적으로 표시되어야 한다. 이를 통해 클라이언트는 응답을 캐시하여 성능을 향상 시킬 수 있다.

- 계층화된 시스템: 아키텍쳐는 계층으로 설계되어야 하며, 각 계층은 부하 분산 또는 보안과 같은 특정 기능을 포함하여 균일한 인터페이스를 구성해야 한다.

<br />

CRUD에 대해 HTTP Method를 대입. Read는 Collection(복수)과 Item(Element)(단수)로 나뉨.

기본 리소스 URL: /products

1. Read (Collection) → GET /products ➡️ 상품 목록 확인

2. Read (Item) → GET /products/{id} ➡️ 특정 상품 정보 확인

3. Create (Collection Pattern 활용) → POST /products ➡️ 상품 추가 (JSON 정보 함께 전달)

4. Update (Item) → PUT 또는 PATCH /products/{id} ➡️ 특정 상품 정보 변경 (JSON 정보 함께 전달)

5. Delete (Item) → DELETE /products/{id} ➡️ 특정 상품 삭제

Thinking in React 예제에 적용하기

```jsx
app.get('/products', (req, res) => {
  const products = [
    {
      category: 'Fruits',
      price: '$1',
      stocked: true,
      name: 'Apple',
    },
    {
      category: 'Fruits',
      price: '$1',
      stocked: true,
      name: 'Dragonfruit',
    },
    {
      category: 'Fruits',
      price: '$2',
      stocked: false,
      name: 'Passionfruit',
    },
    {
      category: 'Vegetables',
      price: '$2',
      stocked: true,
      name: 'Spinach',
    },
    {
      category: 'Vegetables',
      price: '$4',
      stocked: false,
      name: 'Pumpkin',
    },
    {
      category: 'Vegetables',
      price: '$1',
      stocked: true,
      name: 'Peas',
    },
  ];

  res.send({ products });
});
```

자바스크립트 Object 넣어주면 자동으로 JSON으로 바꿔줌!

```jsx
res.send(JSON.stringify({ products })); // -> 오히려 헷갈림
```
