# useRouter

- 클라이언트 컴포넌트에서 라우터를 변경할 때 사용

```tsx
'use client';

import { useRouter } from 'next/navigation';

export default function Page() {
  const router = useRouter();

  return (
    <button type='button' onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  );
}
```

<br />

### `useRouter.push()`

- `router.push(href: string, { scroll: boolean })`
- 브라우저 히스토리 스택에 새 항목을 추가하면서 라우터를 이동한다.

### `useRouter.replace()`

- `router.replace(href: string, { scroll: boolean })`
- 브라우저 히스토리 스택에 항목을 추가하지 않고 라우터를 이동한다.

### `router.refresh()`

- 현재 경로를 새로고침한다.
- 서버에 새로 요청하고, 데이터 리패칭한 다음 서버 컴포넌트를 리렌더링한다.
- 클라이언트는 영향을 받지 않은 리액트(useState) 또는 브라우저 상태(스크롤 위치)를 잃지 않고 서버 컴포넌트 페이로드를 병합한다.

### `router.back()`

- 브라우저 히스토리 스택에 있는 이전 라우터로 이동한다.

### `router.foward()`

- 브라우저의 히스토리 스택에서 다음 페이지로 이동합니다.

<br />

**⚙️ Examples**

1. Router events

   - `usePathname` 과 `useSearchParams` 과 같은 다른 훅을 사용해 페이지 변경 사항을 수신할 수 있다.

   ```tsx
   'use client'

   import { useEffect } from 'react'
   import { usePathname, useSearchParams } from 'next/navigation'

   export function NavigationEvents() {
   	const pathname = usePathname();
     const searchParams = useSearchParams();

   	useEffect(() => {
   		const url = `${pathname}?${searchParms}`
   		console.log(url);
   	), [pathname, searchParams])

   	return null;
   }
   ```

```jsx
import { Suspense } from 'react';
import { NavigationEvents } from './components/navigation-events';

export default function Layout({ children }) {
  return (
    <html lang='en'>
      <body>
        {children}

        <Suspense fallback={null}>
          <NavigationEvents />
        </Suspense>
      </body>
    </html>
  );
}
```

- `<NavigationEvents>`가 `Suspense` boundary로 래핑되는 이유
  - `useSearchParams()` 는 정적 렌더링 중에 가장 가까운 `Suspense` 경계까지 클라이언트 렌더링을 수행하기 때문

1. Disabling scroll resoration

   - Next.js는 기본적으로 새 경로로 이동할 때 페이지 상단으로 스크롤 된다.
   - 이 동작을 비활성화하기 위해서는 `router.push()` 또는 `router.replace()`에 `scroll:false`를 전달해야한다.

   ```jsx
   'use client';

   import { useRouter } from 'next/navigation';

   export default function Page() {
     const router = useRouter();

     return (
       <button
         type='button'
         onClick={() => router.push('/dashboard', { scroll: false })}
       >
         Dashboard
       </button>
     );
   }
   ```
