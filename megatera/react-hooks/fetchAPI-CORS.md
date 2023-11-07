# Fetch API & CORS

## 학습 키워드

* Fetch API 란
* Promise
* ReableStream
* Unicode
* CORS 란



## Fetch API

Fetch API는 원래 웹 브라우저에서 사용하는 API이다. 하지만 Fetch API는 Node.js에서도 사용할 수 있다. Node.js 환경에서는 일반적으로 `node-fetch`와 같은 외부 패키지를 사용하여 Fetch API의 기능을 제공한다.

\


**기본적인 사용법 실험**

```jsx
fetch('http://localhost:3000/products');
// → Promise
```

fetch 함수는 Promise를 반환한다.

```jsx
await fetch('http://localhost:3000/products');
// → Response
```

await 키워드를 사용하면 Response를 반환한다.

```jsx
const response = await fetch('http://localhost:3000/products');
// → response.body는 ReadableStream

const reader = response.body.getReader();
const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입.
// → 원래는 chunk.done이 true일 때까지 반복해야 한다.
```

![.](images/2023-03-28-20-40-28.png)

products는 434바이트짜리 데이터

![](images/2023-05-25-17-51-51.png)

처음 chunk 값은 {value: Uint8Array(434), done: false}, 두 번째 chunk 값은 {value: undefined, done: true}이다. done이 true가 될 때까지 반복한다. 왜 이런 작업을 반복할까?

데이터를 요청했을때 모든 데이터 한 번에 가져오는 것이 아니라, 데이터를 잘게 조각내서 받아오기 때문에 처음 done은 false이고, 다 받아왔다면 done이 true로 변경된다. 그 동안 value값을 계속해서 push해 놓는다.

\


chunk.value는 bite array. 이 데이터를 string으로 바꿔줄 필요가 있다. 👉🏻 텍스트 디코더

```jsx
const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

fetch 함수는 JSON을 기본 지원한다.

```jsx
const response = await fetch('http://localhost:3000/products');
const data = await response.json();
```

다른 HTTP Method를 쓰고 싶다면?

```jsx
const response = fetch(url, {
  method: 'POST',
});
```

객체 안에는 여러가지를 써 줄 수 있음

```jsx
const respose = await fetch(url, {
  method: 'POST',
  mode: 'cors',
  cache: 'no-cache',
  headers: {
    'Content-Type': 'application/json', // - 이 형태는 자주 사용함
  },
});

return response.json(); // JSON 응답을 네이티브 JavaScript 객체로 파싱
```

\


## CORS

* [동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin\_policy)
* [CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

이전에 작업하던 react-demo-app 프로젝트로 돌아가서 main.tsx 코드 수정하기 받아온 data.products에는 뭐가 들어올까?

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import App from './App';

async function main() {
  // fetch
  const url = 'http://localhost:3000/products';
  const response = await fetch(url);
  const data = await response.json();
  const { products } = data;

  console.log(products);

  const container = document.getElementById('root');
  if (!container) {
    return;
  }

  const root = ReactDOM.createRoot(container);
  root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  );
}

main();
```

콘솔창을 확인해보면 보이는 오류 ![](images/2023-03-29-00-21-14.png)

{% hint style="warning" %}
#### Access to fetch at 'http://localhost:3000/products' from origin 'http://localhost:8080' has been blocked by CORS policy:

웹 브라우저가 가지고 있는 기본 보안 정책\
중요한 건 '웹 브라우저가 가지고 있는 것'
{% endhint %}

* 지금 웹 페이지는? 8080
* 리소스를 요청한 곳은? REST API 서버는 3000

웹 브라우저는 Same Origin Policy에 따라 웹 페이지와 리소스를 요청한 곳(여기서는 REST API 서버)이 서로 다른 출처(실제로 포트 번호까지 포함해서 출처가 된다)일 때 서버에서 얻은 결과를 사용할 수 없게 막는다. 서버에 요청하고 응답을 받아오는 것까지는 이미 진행이 다 된 상황이란 점에 주의!

![.](images/2023-03-29-00-27-28.png) 👉🏻 응답을 받아오는 것까지 이미 완료된 상태. 브라우저에서 막아놓는 것!

\


{% hint style="info" %}
#### CORS를 사용해 교차 출처 접근을 허용한다.

'리소스 문제 있지만 나는 괜찮아'라는 것을 서버에서 알려줘야한다.

REST API 서버에서 Headers에 “Access-Control-Allow-Origin” 속성을 추가하면 된다.

Express에선 그냥 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html)를 설치해서 사용하면 됨.
{% endhint %}

\


패키지 설치

```jsx
npm i cors

npm i -D @types/cors
```

CORS 미들웨어 사용

```jsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```
