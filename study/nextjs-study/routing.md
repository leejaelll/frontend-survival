---
description: 웹 라우팅의 기본 개념과 Next.js에서 라우팅을 처리하는 방법
---

# Routing

### Pages and Layout

- App router에는 페이지, 공유 레이아웃, 템플릿을 쉽게 만들 수 있는 파일 규칙이 도입되었다.

<br />

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
