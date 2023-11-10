# useSearchParams

- 현재 URL의 query string을 확인할 수 있는 클라이언트 컴포넌트 훅

```tsx
'use client';

import { useSearchParams } from 'next/navigation';

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParams.get('search');

  // URL -> `/dashboard?search=my-project`
  // `search` -> 'my-project'
  return <>Search: {search}</>;
}
```

---

### Returns

`useSearchParams`는 query string을 읽기 위한 유틸리티 메서드를 포함하는 읽기 전용 버전의 URLSearchParams 인터페이스를 반환한다.

**`URLSearchParams.get()`**

- search parameter와 연관된 첫 번째 값을 반환한다.

| URL                | searchParams.get("a")                |
| ------------------ | ------------------------------------ |
| /dashboard?a=1     | '1'                                  |
| /dashboard?a=      | ''                                   |
| /dashboard?b=3     | null                                 |
| /dashboard?a=1&a=2 | '1' - use getAll() to get all values |

**`URLSearchParams.has()`**

- parameter가 존재하는지에 따라 boolean 값을 반환한다.

| URL            | searchParams.has("a") |
| -------------- | --------------------- |
| /dashboard?a=1 | true                  |
| /dashboard?b=3 | false                 |

---

### Behavior

**🚧 Static Rendering**

- 경로가 정적으로 렌더링되는 경우, `useSearchParams()`를 호출하면 가장 가까운 `Suspense` 경계까지의 트리가 클라이언트 측에서 렌더링된다.
- 이렇게 하면 페이지의 일부가 정적으로 렌더링되는 동안 *searchParams를 사용하는 동적 부분이 클라이언트 측에서 렌더링*된다.
- `Suspense` 경계에서 `useSearchParams`를 사용하는 컴포넌트를 래핑하여 클라이언트 측에서 렌더링되는 경로의 일부를 줄일 수 있다.

`app/dashboard/search-bar.tsx`

```tsx
'use client';

import { useSearchParams } from 'next/navigation';

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParmas.get('search');

  // This will not be logged on the server when using static rendering
  console.log(search);

  return <>Search: {search}</>;
}
```

`app/dashboard/page.tsx`

```tsx
import { Suspense } from 'react';
import SearchBar from './search-bar';

// 서스펜스 경계에 대한 폴백으로 전달된 이 컴포넌트는
// 초기 HTML에서 검색창 대신 렌더링됩니다.
// 리액트 하이드레이션 중에 값을 사용할 수 있으면 폴백
// `<SearchBar>` 컴포넌트로 대체됩니다.

function SearchBarFallback() {
  return <>placeholder</>;
}

export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

<br />

**🚧 Static Rendering**

- 경로가 동적으로 렌더링되는 경우, 클라이언트 컴포넌트의 초기 서버 렌더링 중에 서버에서 `useSearchParams`를 사용할 수 있다.

`app/dashboard/search-bar.tsx`

```tsx
'use client';

import { useSearchParams } from 'next/navigation';

export default function SearchBar() {
  const searchParams = useSearchParams();

  const search = searchParams.get('search');

  // This will be logged on the server during the initial render
  // and on the client on subsequent navigations.
  console.log(search);

  return <>Search: {search}</>;
}
```

`app/dashboard/page.tsx`

```tsx
import SearchBar from './search-bar';

export const dynamic = 'force-dynamic'; // ⛏️레이아웃 또는 페이지의 동적 동작을 완전 정적 또는 완전 동적으로 변경하는 옵션

export default function Page() {
  return (
    <>
      <nav>
        <SearchBar />
      </nav>
      <h1>Dashboard</h1>
    </>
  );
}
```

<br />

**🚧 Server Components**

1. Pages

- 페이지에서 search params 접근하려면 searchParams를 사용한다.

2. Layouts

- 페이지와 달리 레이아웃은 search params를 받지 않는다.
- 탐색 중에 공유 레이아웃이 리렌더링 되지 않아 search params가 부실해질 수도 있기 때문
- 대신 클라이언트 컴포넌트에서 Page `searchParams`프로퍼티 또는 `useSearchParams` 훅을 사용하면 최신 `SearchParams로` 리렌더링 된다.

---

### Examples

**✅ Updating `searchParams`**

- naviation이 수행된 이후에 현재 `page.js`는 업데이트된 searchParmas 프로퍼티를 받게된다.

`app/example-client-component.tsx`

```tsx
export default function ExampleClientComponent() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams()!;

  // 현재 검색 매개변수를 제공된 키/쌍과 병합하여 새로운 검색 매개변수 문자열을 가져옵니다.
  // searchParams를 제공된 키/값 쌍과 병합하여 새로운 검색 매개변수 문자열을 가져옵니다.
  const createQueryString = useCallback(
    (name: string, value: string) => {
      const params = new URLSearchParams(searchParams);
      params.set(name, value);

      return params.toString();
    },
    [searchParams]
  );
}

return (
  <>
    <p>Sort By</p>

    {/* using useRouter */}
    <button
      onClick={() => {
        // <pathname>?sort=asc
        router.push(pathname + '?' + createQueryString('sort', 'asc'));
      }}
    >
      ASC
    </button>

    {/* using <Link> */}
    <Link
      href={
        // <pathname>?sort=desc
        pathname + '?' + createQueryString('sort', 'desc')
      }
    >
      DESC
    </Link>
  </>
);
```
