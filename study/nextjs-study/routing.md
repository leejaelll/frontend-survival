---
description: 웹 라우팅의 기본 개념과 Next.js에서 라우팅을 처리하는 방법
---

# Routing

### Pages and Layout

- App router에는 페이지, 공유 레이아웃, 템플릿을 쉽게 만들 수 있는 파일 규칙이 도입되었다.

**✅ Pages**

- 페이지는 고유의 UI
- page.js 파일에서 컴포넌트를 내보내 페이지를 정의할 수 있다.
- 중첩 폴더를 사용하여 경로를 정의하고, page.js 파일을 사용하여 경로를 공개적으로 액세스 할 수 있다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fpage-special-file.png&w=3840&q=75&dpl=dpl_4qFgWDBnr2BXaLLxBUyHvuoqxZtg" alt=""><figcaption></figcaption></figure>

- `app/page.tsx` 👉🏻 UI for the `/` URL
- `app/dashbord/page.tsx` 👉🏻 UI for the `/dashboard` URL

<br />

**✅ Layouts**

- 레이아웃은 여러 페이지 간에 공유되는 UI
  - navigation에서 레이아웃은 상태를 보존하고, interactive를 유지하며, 리렌더링을 하지 않는다.
  - 레이아웃은 중첩될 수도 있다.
- layout.js 파일에서 React component를 default exporting을 통해 정의할 수 있다.
  - 컴포넌트는 렌더링 중에 자식 레이아웃 또는 자식 페이지로 채워질 자식 프로퍼티를 받아야 한다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Flayout-special-file.png&w=3840&q=75&dpl=dpl_4qFgWDBnr2BXaLLxBUyHvuoqxZtg" alt=""><figcaption></figcaption></figure>

```typescript
export default function DashboardLayout({
  children, // 페이지 또는 중첩 레이아웃
}: {
  children: React.ReactNode;
}) {
  return (
    <section>
      {/* 헤더 또는 사이드바 등 공유 UI를 여기에 포함 */}
      <nav></nav>

      {children}
    </section>
  );
}
```

{% hint style="info" %}

- top-most layout을 Root Layout이라고 한다.
  - required layout
  - 애플리케이션의 모든 페이지에서 공유된다.
  - HTML 및 본문 태그가 포함되어야 한다.
- 부모 레이아웃과 자식 레이아웃 간에 데이터를 전달할 수 없다.
  - 경로에서 동일한 데이터를 두 번 가져올 수 있음.
  - React는 성능에 영향을 주지 않고 요청을 자동으로 중복 제거한다.

{% endhint %}

**💡 Root Layout**

- required
- 루트 레이아웃은 top level에 정의되며 모든 경로에 적용된다.

```typescript
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang='en'>
      <body>{children}</body>
    </html>
  );
}
```

**💡 Nesting Layouts**

- 폴더 내에 정의된 레이아웃은(`app/dashboard/layout.js`) 특정 route segment(`acme.com/dashboard`)에 적용되며 해당 세그먼트가 활성화될 때 렌더링된다.
- 기본적으로 파일 계층 구조의 레이아웃은 중첩되어 있으므로 자식 프로퍼티를 통해 자식 레이아웃을 감싸게 된다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fnested-layout.png&w=3840&q=75&dpl=dpl_4qFgWDBnr2BXaLLxBUyHvuoqxZtg" alt=""><figcaption></figcaption></figure>

```typescript
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return <section>{children}</section>;
}
```

<br />

**✅ Templates**

- 템플릿은 하위 레이아웃 또는 페이지를 래핑한다는 점에서 레이아웃과 유사하다.
- 경로 전체에서 지속되고 상태를 유지하는 레이아웃과는 달리 템플릿은 탐색 시, 각 하위 레이아웃에 대해 새 인스턴스를 생성한다.
- 즉, 사용자가 템플릿을 공유하는 경로 사이를 탐색할 때 컴포넌트의 새 인스턴스가 마운트되고, DOM 요소가 다시 생성되며, 상태가 보존되지 않고, effects가 다시 동기화된다.

---

### Linking and Navigating

Next.js에서 경로를 탐색하는 두 가지 방법

1. `<Link>` 컴포넌트
2. `useRouter` Hook

<br />

**✅ `<Link>` Component**

- `<Link>`는 `<a>` 태그를 확장하여 경로 간에 클라이언트 사이드 네비게이션과 프리페칭을 제공하는 built-in component
- `next/link`에서 가져온 후, href 프로퍼티를 전달하여 사용할 수 있다.

```typescript
import Link from 'next/link';

export default function Page() {
  return <Link href='/dashboard'>Dashboard</Link>;
}
```

**⚙️ Examples**

1. Linking to Dynamic Segments

   - 동적 세그먼트에 링크할 때, 템프릿 리터럴 및 interpolation을 사용하여 링크 목록을 생성할 수 있다.
   - 블로그 게시물 목록 생성 예시:

   ```typescript
   import Link from 'next/link';

   export default function PostList({ posts }) {
     return (
       <ul>
         {posts.map((post) => (
           <li key={post.id}>
             <Link href={`/blog/${post.slug}`}>{post.title}</Link>
           </li>
         ))}
       </ul>
     );
   }
   ```

2. Checking Active Links

   - `usePathname()`을 사용하여 링크가 활성 상태인지 확인할 수 있다.
   - 활성 링크에 클래스를 추가하려면 현재 경로명이 링크의 href와 일치하는지 확인할 수 있다.

   ```typescript
   'use client';

   import { usePathname } from 'next/navigation';
   import Link from 'next/link';

   export function Links() {
     const pathname = usePathname();

     return (
       <nav>
         <ul>
           <li>
             <Link
               className={`link ${pathname === '/' ? 'active' : ''}`}
               href='/'
             >
               Home
             </Link>
           </li>
           <li>
             <Link
               className={`link ${pathname === '/about' ? 'active' : ''}`}
               href='/about'
             >
               About
             </Link>
           </li>
         </ul>
       </nav>
     );
   }
   ```

3. Scrolling to an id

   - Next.js App Router의 기본 동작은 새 경로의 상단으로 스크롤하거나 뒤로 및 앞으로 탐색을 위해 스크롤 위치를 유지하는 것
   - 네비게이션에서 특정 id로 스크롤하려면 URL에 # 해시 링크를 추가하거나 href 프로퍼티에 해시 링크를 전달하면 된다.

   ```typescript
     <Link href="/dashboard#settings">Settings</Link>

   // Output
   <a href="/dashboard#settings">Settings</a>
   ```

4. Disabling scroll restoration

   - App router의 기본 동작을 비활성화하려면 `<Link>` 컴포넌트에 `scroll={false}`를 전달하거나, `router.push()` 또는 `router.replace()`를 전달할 수 있다.

   ```typescript
   // useRouter
   import { useRouter } from 'next/navigation';

   const router = useRouter();
   router.push('/dashboard', { scroll: false });
   ```

<br />

**✅ `<useRouter()>` Hook**

- useRouter 훅은 프로그래밍 방식으로 라우터를 변경할 수 있다.
- 클라이언트 컴포넌트 내에서만 사용 가능하며, `next/navigation`에서 import 해온다.

```typescript
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

**✅ How Routing and Navigation Works**

- 서버에서는 애플리케이션 코드가 경로 세그먼트별로 자동으로 코드 분할함.
- 클라이언트에서는 Next.js가 경로 세그먼트를 프리페치하고 캐싱함.
- 사용자가 새 경로로 이동할 때 브라우저가 페이지를 새로고침하지 않고 변경된 경로 세그먼트만 다시 렌더링하여 내비게이션 환경과 성능을 개선한다.

1. Prefetching
   - 사용자가 경로를 방문하기 전에 백그라운드에서 경로를 미리 로드하는 방법
   - Next.js에서 prefetch하는 두 가지 방법
     - `<Link>` component
     - `router.prefetch()`: `useRouter` hook
   - `<Link>`의 프리패칭은 정적 라우터와 동적 라우터에 따라 다르게 동작한다.
     - Static Routes: `prefetch`가 기본적으로 true, 전체 경로가 프리페치되고 캐시된다.
     - Dynamic Routes: 기본값은 자동, 첫 번째 loading.js 파일까지의 공유 레이아웃만 프리페치되고 30초 동안 캐시된다. 👉🏻 이렇게 하면 전체 동적 경로를 불러오는 데 드는 비용이 절감되며, 로딩 상태를 즉시 표시하여 사용자에게 더 나은 시각적 피드백을 제공할 수 있다.
2. Caching
   - Next.js에는 라우터 캐시라는 인메모리 클라이언트 측 캐시가 있다.
   - 사용자가 앱을 탐색할 때 미리 가져온 경로 세그먼트와 방문한 경로의 React 서버 컴포넌트 페이로드가 캐시에 저장된다.
3. Partial Rendering
   - 내비게이션에서 변경된 경로 구간만 클라이언트에서 다시 렌더링하고 공유 구간은 그대로 유지되는 것을 의미
   - 예를 들어, 두 형제 경로인 /dashboard/settings와 /dashboard/analytics 사이를 탐색하는 경우 설정 및 분석 페이지가 렌더링되고 공유 대시보드 레이아웃은 유지된다.
   <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fpartial-rendering.png&w=3840&q=75&dpl=dpl_HDakJ5aJAjrXzcb8oDPfbnna9n6L" alt=""><figcaption></figcaption></figure>
4. Soft Navigation
   - 기본적으로 브라우저는 페이지를 다시 로드하고, 앱의 **useState와 같은 리액트 상태**와 **사용자의 스크롤 위치나 포커스된 요소와 같은 브라우저 상태**를 초기화한다.
   - 하지만 Next.js에서 앱 라우터는 소프트 내비게이션을 사용한다. 즉, React는 React와 브라우저 상태를 유지하면서 변경된 세그먼트만 렌더링하며 전체 페이지를 다시 로드하지 않는다.
5. Back and Forward Navigation
   - 기본적으로 Next.js는 뒤로 및 앞으로 탐색을 위한 스크롤 위치를 유지하고 라우터 캐시에서 경로 세그먼트를 재사용한다.

---

### Route Groups

- 중첩된 폴더는 URL 경로에 매핑되지만, 폴더를 Route Group으로 표시하여 폴더가 URL 경로에 포함되지 않도록 할 수 있다.
- 이를 통해 URL 경로 구조에 영향을 주지 않고, 경로 세그먼트와 프로젝트 파일을 논리적 그룹으로 구성할 수 있다.

<br />

**✅ Convention**

`(folderName)` 과 같이 괄호로 묶어서 route group을 만들 수 있다.

<br />

**⚙️ Examples**

1. URL 경로에 영향을 주지 않고 route 구성하기

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-organisation.png&w=1920&q=75&dpl=dpl_A5LhRZU7CiHuU4wcVEzVnuYFVuZS" alt=""><figcaption></figcaption></figure>

- `(marketing)`, `(shop)` 내부의 경로가 동일한 URL 계층 구조를 공유하더라도 해당 폴더에 layout.js 파일을 추가하여 각 그룹에 대해 서로 다른 레이아웃을 만들 수 있다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-multiple-layouts.png&w=1920&q=75&dpl=dpl_A5LhRZU7CiHuU4wcVEzVnuYFVuZS" alt=""><figcaption></figcaption></figure>

2. 특정 세그먼트를 레이아웃에 선택하기

- 특정 경로를 레이아웃으로 선택하려면 새 route group을 생성하고 동일한 레이아웃을 공유하는 route를 그룹으로 이동시킨다. 그룹 외부의 경로는 레이아웃을 공유하지 않는다.
- 예를 들어, (shop), (account), (cart)는 동일한 레이아웃을 공유하지만 checkout은 공유하지 않는다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-opt-in-layouts.png&w=1920&q=75&dpl=dpl_A5LhRZU7CiHuU4wcVEzVnuYFVuZS" alt=""><figcaption></figcaption></figure>

3. 여러 루트 레이아웃 만들기

- root layout을 여러 개 만들려면, 최상위 layout.js 파일을 제거하고 각 경로 그룹에 layout.js 파일을 추가한다.
- 이는 애플리케이션을 완전히 다른 UI 또는 UX를 가진 섹션으로 분할하는데 도움이 된다.
- 각 root layout에 `<html>`, `<body>` 태그를 추가해야 한다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-multiple-root-layouts.png&w=1920&q=75&dpl=dpl_A5LhRZU7CiHuU4wcVEzVnuYFVuZS" alt=""><figcaption></figcaption></figure>

---

### Dynamic Routes

- 동적 라우트를 생성하려는 경우, 요청 시 채워지거나, 빌드 시 미리 렌더링되는 동적 세그먼트를 사용할 수 있다.
  <br />

**✅ Convention**

`[folderName]`, `[id]`, `[slug]`와 같이 대괄호로 묶어 만들 수 있다.

- 동적 세그먼트는 `layout`, `page`, `route` 및 `generatemetadata` 함수에 매개변수로 전달된다.

<br />

**⚙️ Examples**

`app/blog/[slug]/page.js`와 같은 라우트를 포함할 때 사용한다.

```tsx
export default function Page({ params }: { slug: string }) {
  return <div>My post: {params.slug}</div>;
}
```

| Route                   | Example URL | params        |
| ----------------------- | ----------- | ------------- |
| app/blog/[slug]/page.js | /blog/a     | { slug: 'a' } |
| app/blog/[slug]/page.js | /blog/b     | { slug: 'b' } |
| app/blog/[slug]/page.js | /blog/c     | { slug: 'c' } |

<br />

**✅ Generating Static Params**

- `generateStaticParams` 함수는 동적 경로 세그먼트와 함께 사용하여 요청 시 on-demand가 아닌 **빌드 시점에 경로를 정적으로 생성할 수 있다.**

````tsx
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}```
````

- `generateStaticParams` 함수의 가장 큰 장점: 데이터를 스마트하게 검색할 수 있다는 점
- fetch 요청을 사용하여 컨텐츠를 가져오면 요청이 자동으로 메모화된다.
- generateStaticParams, Layouts, Pages에 걸쳐 동일한 인자가 포함된 fetch 요청은 한 번만 수행되므로 빌드시간이 단축된다.

<br />

**✅ Catch-all Segments**

- 동적 세그먼트는 괄호 안에 줄임표`[...folderName]`를 추가하여 안에 모든 후속 세그먼트를 포함하도록 확장할 수 있다.
- 예를 들어, `app/shop/[...slug]/page.js`는 `/shop/clothes` 뿐만 아니라 `/shop/clothes/tops`, `shop/clothes/tops/t-shirts` 등과도 일치한다.

| Route                      | Example URL | params                    |
| -------------------------- | ----------- | ------------------------- |
| app/shop/[...slug]/page.js | /shop/a     | { slug: ['a'] }           |
| app/shop/[...slug]/page.js | /shop/a/b   | { slug: ['a', 'b'] }      |
| app/shop/[...slug]/page.js | /shop/a/b/c | { slug: ['a', 'b', 'c'] } |

<br />

**✅ Optional Catch-all Segments**

- Catch-all Segments는 매개변수를 이중 대괄호 안에 포함하여 선택사항으로 만들 수 있다. 👉🏻 `[[...folderName]]`
- 예를 들어, `app/shop/[[...slut]]/page.js`는 `/shop`, `/shop/clothes`, `/shop/clothes/tops`과 일치한다.

{% hint style="info" %}

### catch-all 과 optional catch-all의 차이점

- optional catch-all을 사용하면 매개변수가 없는 경로도 일치한다는 것이 차이점

| Route                        | Example URL | params                    |
| ---------------------------- | ----------- | ------------------------- |
| app/shop/[[...slug]]/page.js | /shop       | {}                        |
| app/shop/[[...slug]]/page.js | /shop/a     | { slug: ['a'] }           |
| app/shop/[[...slug]]/page.js | /shop/a/b   | { slug: ['a', 'b'] }      |
| app/shop/[[...slug]]/page.js | /shop/a/b/c | { slug: ['a', 'b', 'c'] } |

{% endhint %}

<br />

**🔥 TypeScript**

- 타입스크립트를 사용할 때, 구성된 경로 세그먼트에 따라 매개변수에 대한 타입을 추가할 수 있다.

| Route                             | params Type Definition                 |
| --------------------------------- | -------------------------------------- |
| app/blog/[slug]/page.js           | { slug: string }                       |
| app/shop/[...slug]/page.js        | { slug: string[] }                     |
| app/[categoryId]/[itemId]/page.js | { categoryId: string, itemId: string } |

---

### Loading UI and Streaming

- 특수 파일 `loading.js`를 사용하면 React Suspense로 의미있는 로딩 UI를 만들 수 있다.
- 이 규칙을 사용하면 경로 세그먼트의 컨텐츠가 로드되는 동안 서버에서 즉각적인 로딩 상태를 표시할 수 있다.

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Floading-ui.png&w=3840&q=75&dpl=dpl_AGrSzwth6hUFvw45h9RdJK62uM3B" alt=""><figcaption></figcaption></figure>

<br />

**🚧 Instant Loading States**

- instant loading state는 탐색 시 즉시 표시되는 fallback UI이다.
- 스켈레톤, 스피너와 같은 로딩 표시기를 미리 렌더링하거나, 표지사진, 제목 등과 같이 작지만 의미있는 부분을 향후 화면에 표시할 수 있다.
- 이를 통해 사용자가 앱이 반응하고 있음을 이해하고 더 나은 UX를 제공할 수 있다.

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Floading-special-file.png&w=3840&q=75&dpl=dpl_AGrSzwth6hUFvw45h9RdJK62uM3B" alt=""><figcaption></figcaption></figure>

```tsx
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />;
}
```

- 같은 폴더에서 loading.js는 layout.js안에 중첩된다.
- page.js 파일과 모든 하위 파일은 자동으로 `<Suspense>` 경계로 감싸진다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Floading-overview.png&w=3840&q=75&dpl=dpl_AGrSzwth6hUFvw45h9RdJK62uM3B" alt=""><figcaption></figcaption></figure>

<br />

**🚧 Streaming with Suspense**

- loading.js 외에도 자체 UI 컴포넌트에 대한 Suspense 바운더리를 수동으로 생성할 수 있다.
- App Router는 Node.js 및 Edge 런타임 모두에 대해 Suspense를 사용한 스트리밍을 지원한다.

{% hint style="success" %}

### What is Streaming?

React와 Next.js에서 스트리밍이 어떻게 동작하는지 알아보려면 SSR과 그 한계를 이해하는 것이 도움이 된다.

**⛏️ SSR을 사용했을 때, 사용자가 페이지를 보고 상호작용하기 전에 완료해야 하는 일련의 단계**

1. 특정 페이지의 모든 데이터를 서버에서 가져온다.
2. 서버에서 페이지의 HTML을 렌더링한다.
3. HTML, CSS, JavaScript가 클라이어트로 전송된다.
4. 생성된 HTML과 CSS를 사용해 비대화형 사용자 인터페이스가 표시된다.
5. 마지막으로 React로 대화형 인터페이스로 만든다. (Hydrating)

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75&dpl=dpl_AGrSzwth6hUFvw45h9RdJK62uM3B" alt=""><figcaption></figcaption></figure>

이러한 단계는 순차적이고 차단적이므로 **서버는 모든 데이터를 가져온 후에만** 페이지의 HTML을 렌더링할 수 있다. 그리고 클라이언트에서는 페이지의 모든 컴포넌트에 대한 코드가 다운로드 된 후에만 React가 hydrate할 수 있다.

React 및 Next.js를 사용한 SSR은 사용자에게 비대화형 페이지를 최대한 빨리 보여줌으로써 체감 로딩을 성능을 개선하는데 도움이 된다.

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fserver-rendering-without-streaming.png&w=3840&q=75&dpl=dpl_AGrSzwth6hUFvw45h9RdJK62uM3B" alt=""><figcaption></figcaption></figure>

{% endhint %}
