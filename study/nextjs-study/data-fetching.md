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

`async/await`와 함께 fetch를 사용한다. (in Server Component, Route Handler, Server Actions)

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

<br />

**Caching Data**

- 캐싱은 데이터를 저장하므로 요청할 때마다 데이터 소스에서 데이터를 다시 가져올 필요가 없다.
- 기본적으로 Next.js는 서버의 데이터 캐시에 fetch 반환값을 자동으로 캐싱한다. 👉🏻 즉, 빌드 시간 또는 요청 시간에 데이터를 가져와서 캐시한 후 각 데이터 요청에서 재사용할 수 있다.

```tsx
// 'force-cache' is the default, and can be omitted
fetch('https://...', { cache: 'force-cache' });
```

**Revalidating Data**

- 재검증(Revalidation)은 데이터를 지우고 최신 데이터를 리패칭하는 과정
- 데이터가 변경되어 최신 정보를 표시하고 싶을 때 유용하다.
- 캐시된 데이터를 재검증할 수 있는 두 가지 방법

  1. Time-based revalidation (시간 기반 재검증)

  - 일정 시간이 경과한 후, 자동으로 데이터의 유효성을 재검증한다.
  - 이 방법은 자주 변경되지 않고, 최신성이 그다지 중요하지 않은 데이터에 유용하다.
  - `next.revalidate` 옵션을 사용하여 리ㅗ스의 캐시 수명을 설정할 수 있다.

    ```tsx
    fetch('https://...', { next: { revalidate: 3600 } });
    ```

  2. On-demand revalidation (온디맨드 재검증)

  - 폼 제출과 같은 이벤트를 기반으로 데이터를 수동으로 재검증한다.
  - 온디맨드 재검증은 태그 기반 또는 경로 기반 접근 방식을 사용하여 데이터 그룹을 한 번에 재검증 할 수 있다.
  - 최신 데이터를 가능한 한 빨리 표시하려는 경우 유용하다.
  - Next.js에는 여러 경로에서 패치 요청을 무효화하기 위한 캐시 태그 시스템이 있다.

    1. fetch를 사용할 때, 하나 이상의 태그로 캐시 항목에 태그를 지정하는 옵션을 사용한다.
    2. 그 다음 `revalidateTag`를 호출하여 해당 태그와 연결된 모든 항목의 유효성을 재검증한다.

    ```tsx
    export default async fnction Page() {
      const res = await fetch('https://...', {next: {tags: ['collection']}})
      const data = await res.json()
    }
    ```

    - 서버 액션에서 revalidateTag를 호출하여 'collection'으로 태그된 패치 호출의 유효성을 재확인할 수 있다.

    ```tsx
    'use server';

    import { revalidateTag } from 'next/cache';

    export default async function action() {
      revalidateTag('collection');
    }
    ```
