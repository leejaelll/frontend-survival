# MSW

## 학습 키워드

* Service worker
* MSW(Mock Service Worker)
* polyfill(폴리필)



* [MSW](https://mswjs.io/)
* [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service\_Worker\_API)
* [아샬의 Mock Service Worker (MSW)](https://github.com/ahastudio/til/blob/main/mock-api/msw.md)
* [Mocking REST API](https://mswjs.io/docs/getting-started/mocks/rest-api)
* [Integrate mocking into Node](https://mswjs.io/docs/getting-started/integrate/node)

{% hint style="info" %}
#### Service worker

서비스 워커는 웹 응용 프로그램, 브라우저, 그리고 (사용 가능한 경우) 네트워크 사이의 프록시 서버 역할을 한다.

* 특정 출처(사이트)의 하나 혹은 그 이상의 페이지를 제어하는 스크립트이며, 이벤트 기반 워커로서 JavaScript로 작성된 파일
* 자신이 제어하는 페이지에서 발생하는 이벤트를 수신할 수 있다.
* 웹에서의 네트워크 요청과 같은 이벤트를 가로채어 수정할 수 있고 이를 다시 페이지로 돌려보낼 수있다.
* 서비스에서 사용하는 리소스를 캐싱할 수 있다.

**서비스 워커가 네트워크 요청을 가로챌 수 있고, 리소스를 캐싱할 수 있는 이유는?**

: 브라우저 또는 탭의 외부에 위치하기 때문

서비스 워커는 브라우저와 네트워크 사이의 새로운 계층에 존재합니다. 따라서, 브라우저와 네트워크 사이의 연결과 관계없이 서비스 워커는 브라우저와의 독립적인 연결을 가진다.

이러한 특징들 덕분에 오프라인 상태일 때 브라우저에 캐시된 리소스를 전달할 수 있다.

* [서비스 워커에 대해 알아보고 Mock Response 만들기](https://fe-developers.kakaoent.com/2022/221208-service-worker/)
{% endhint %}

```tsx
import { setupWorker, rest } from 'msw';
interface LoginBody {
  username: string;
}
interface LoginResponse {
  username: string;
  firstName: string;
}
const worker = setupWorker(
  rest.post<LoginBody, LoginResponse>('/login', async (req, res, ctx) => {
    const { username } = await req.json();
    return res(
      ctx.json({
        username,
        firstName: 'John',
      })
    );
  })
);
worker.start();
```

👉🏻 express 구조와 비슷하게 되어있다.

이전까지는 코드 레벨에서 moking을 했다면, MSW는 네트워크 레벨에서 가짜 구현. 오프라인 작업 등을 지원하기 위한 서비스 워커의 기능을 유용히 활용한 것.

MSW 패키지 설치

```bash
npm i -D msw
```

`jest.config.js` 파일의 “setupFilesAfterEnv” 속성에 `setupTests.ts` 파일 추가.

```jsx
module.exports = {
  testEnvironment: 'jsdom',
  **setupFilesAfterEnv**: [
    '@testing-library/jest-dom/extend-expect',
    '**<rootDir>/src/setupTests.ts**',
  ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          jsx: true,
          decorators: true,
        },
        transform: {
          react: {
            runtime: 'automatic',
          },
        },
      },
    }],
  },
};
```

`src/setupTests.ts` 파일

```jsx
import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

`src/mocks/server.ts` 파일

```jsx
import { setupServer } from 'msw/node';

import handlers from './handlers';

const server = setupServer(...handlers);

export default server;
```

`src/mocks/handlers.ts` 파일

→ Express의 경험을 살려보자!

```jsx
import { rest } from 'msw';

const BASE_URL = 'http://localhost:3000';

const handlers = [
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
    const products = [
      {
        category: 'Fruits',
        price: '$1',
        stocked: true,
        name: 'Apple',
      },
    ];

    return res(ctx.status(200), ctx.json({ products }));
  }),
];

export default handlers;
```

`App.test.ts` 파일

```jsx
import { render, screen, waitFor } from '@testing-library/react';

import App from './App';

// jest.mock 불필요.

test('App', async () => {
  render(<App />);

  await waitFor(() => {
    screen.getByText('Apple');
  });
});
```

\


원래 로딩을 하면 시간이 걸리는데 테스트에서는 바로 체크하기 때문에 값을 가져올 수 없다는 오류를 발생시킨다.

나는 조금 기다려줬으면 좋겠는데..?할 땐 `waitFor()`을 사용하면된다. (테스트계의 프로미스..?)

```tsx
waitFor(() => {
  screen.getByText('Apple');
});
```

waitFor 명세서에 들어가보면 promise로 반환하는 것을 확인할 수 있음. 그래서 `async`와 `await`를 붙여야 하는 것!

```tsx
import { render, screen, waitFor } from '@testing-library/react';

import App from './App';

// jest.mock 불필요.

**test('App', async () => {
  render(<App />);

  await waitFor(() => {
    screen.getByText('Apple');
  });
});**
```

\


너무 본격적으로 코딩하면 사실상 백엔드를 개발하게 되니, 이 부분에 주의할 것.

테스트 환경(Node.js 기반) 외에 웹 브라우저도 지원하기 때문에, API 스펙은 나왔지만 아직 구현되지 않은 경우 임시로 사용할 수도 있다.

단순히 임시 서버를 만들 거라면 Express를 쓰는 게 더 낫지만, 테스트 코드도 지원하면서 겸사겸사 웹 브라우저를 지원하는 용도로는 나쁘지 않은 선택이다.

```tsx
npm i -D whatwg-fetch
```

\


whatwg-fetch를 사용하려면 `import ‘whatwg-fetch’`를 문서 최상단에 써줘야하는데 모든 테스트 파일에 써줄 순 없으니, `setupTests.ts`에 한번 import 해주어 프록시 적용받는 것에 다 적용될 수 있도록 한다.

```tsx
import 'whatwg-fetch';

import server from './mocks/server';

beforAll(() => server.listen({ onUnhandledRequest: 'error' }));
afterAll(() => server.close());
afterEach(() => server.restHandlers());
```

* [GitHub에서 만든 fetch polyfill](https://github.com/github/fetch)
