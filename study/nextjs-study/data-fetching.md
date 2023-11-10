---
description: Fetching, Caching, Forms and Mutation
---

# Data Fetching

### Data Fetching, Caching, and Revalidating

> data fetching은 모든 애플리케이션의 핵심부분이다. 이 페이지에서는 React와 Next.js에서 데이터를 불러오고, 캐시하고, 재검증하는 방법에 대해서 설명한다.

<br />

{% hint style="info" %}

**_데이터를 패치하는 4가지 방법_**

1. 서버에서 fetch 사용
2. 서버에서 써드파티 라이브러리 사용
3. 클라이언트에서 Route Handler 사용
4. 클라이언트에서 써드파티 라이브러리 사용

{% endhint %}

<br />

**💡 Fetching Data on the Server with fetch**

- Next.js는 `fetch` Web API를 확장하여 fetch 요청에 대한 캐싱 및 재검증 동작을 구성할 수 있도록 한다.
- React는 fetch를 확장하여 React 컴포넌트 트리를 렌더링하는 동안 fetch 요청을 자동으로 메모화한다.

async/await와 함께 fetch를 사용한다. (in Server Component, Route Handler, Server Actions)

```tsx
async function getData() {
  const res = await fetch('https://api.example.com/...');

  if (!res.ok) {
    // This will activate the closest `error.js` Error Boundary
    throw new Error('Failed to fetch data');
  }

  return res.json();
}

export default async function Page() {
  const data = await getData();

  return <main></main>;
}
```
