# Rendering

### Server Components

**🔥 Benefits of Server Rendering**

1. 데이터 패칭

- 렌더링에 필요한 데이터를 가져오는데 걸리는 시간과 클라이언트가 요청해야 하는 양을 줄여 성능을 향상시킬 수 있다.

2. 보안

- 토큰이나 API 키와 같은 데이터와 로징을 클라이언트에 노출할 위험 없이 서버에 보관할 수 있다.

3. 캐싱

- 각 요청에서 수행되는 렌더링 및 데이터 패칭 양을 줄여 성능을 개선하고 비용을 절감할 수 있다.

4. 번들 크기

- 클라이언트가 서버 컴포넌트용 JavaScript를 다운로드, 파싱, 실행할 필요가 없으므로 인터넷 속도가 느리거나 성능이 낮은 기기를 사용하는 사용자에게 유리하다.

5. 초기 페이지 로드 및 첫 번째 컨텐츠 페인트(LCP)

- 서버에서는 클라이언트가 페이지를 렌더링하는데 필요한 JavaScript를 다운로드, 파싱 및 실행까지 기다릴 필요 없이 사용자가 즉시 페이지를 볼 수 있도록 HTML을 생성할 수 있다.

6. SEO 및 소셜 네트워크 공유가능성

- 렌더링된 HTML은 검색 엔진 봇이 페이지 인덱스를 생성하는데 사용할 수 있으며, 소셜 네트워크 봇이 페이지에 대한 소셜 카드 미리보기를 생성하는데 사용할 수 있다.

7. 스트리밍

- 서버 컴포넌트를 사용하려면 렌더링 작업을 청크로 분할하여 준비되는 대로 클라이언트로 스트리밍할 수 있다. 이를 통해 사용자는 서버에서 전체 페이지가 렌더링될 때까지 기다릴 필요없이 페이지의 일부를 먼저 볼 수 있다.

<br />

**🔥 How are Server Components rendered?**

서버에서 Next.js는 React의 API를 사용하여 렌더링을 지휘한다.렌더링 작업은 개별 경로 세그먼트와 서스펜스 바운더리에 따라 청크로 분할된다.

각 청크는 두 단계로 렌더링된다.

1. React는 서버 컴포넌트를 React 서버 컴포넌트 페이로드(RSC Payload)라는 특수 데이터 형식으로 렌더링한다.
2. Next.js는 RSC 페이로드와 클라이언트 컴포넌트 자바스크립트 명령어를 사용해 서버에서 HTML을 렌더링한다.

그리고 클라이언트에서는,

1. HTML은 경로의 빠른 비대화형 프리뷰를 즉시 표시하는데 사용되며, 이는 초기 페이지 로드에만 사용된다.
2. React 서버 컴포넌트 페이로드는 클라이언트 및 서버 컴포넌트 트리를 조정하고, DOM을 업데이트하는데 사용된다.
3. 자바스크립트 명령어는 클라이언트 컴포넌트를 채우고 애플리케이션을 대화형으로 만드는데 사용된다.

<br />

**🔥 Server Rendering Strategies**

**✅ Static Rendering**

정적 렌더링을 사용하면 빌드 시 또는 데이터 재검증 후 백그라운드에서 경로가 렌더링된다.

_<mark style="color:red;">
👉🏻 정적 렌더링은 정적 블로그 게시물이나 제품 페이지와 같이 경로에 사용자에 맞춤화되지 않고 빌드 시점에 알 수 있는 데이터가 있는 경우에 유용하다.
</mark>_

<br />

**✅ Dynamic Rendering**

동적 렌더링을 사용하면 요청 시점에 각 사용자에 대한 경로가 렌더링된다.

_<mark style="color:red;">
👉🏻 동적 렌더링은 경로에 사용자에게 맞춤화된 데이터가 있거나 쿠키 또는 URL의 검색 매개변수와 같이 요청 시점에만 알 수 있는 정보가 있는 경우 유용하다.
</mark>_

{% hint style="info" %}

### Dynamic Functions

동적 함수는 사용자의 쿠키, 현재 요청 헤더 또는 URL 검색 매개변수와 같이 요청 시점에만 알 수 있는 정보에 의존한다.

- `cookies()`, `headers()` : 서버 컴포넌트에서 사용하면 요청 시, 전체 경로가 동적 렌더링으로 전환된다.
- `useSearchParams()` : 클라이언트 컴포넌트에서는 정적 렌더링을 건너뛰고 대신 클라이언트에서 가장 가가운 부모 서스펜스 경계까지 모든 클라이언트 컴포넌트를 렌더링한다.

`useSearchParams`를 사용하는 클라이언트 컴포넌트를 `<Suspense>`로 감싸놓는 것이 좋다. 👉🏻 그 위에 모든 클라이언트 컴포넌트가 정적으로 렌더링될 수 있다.

- `searchParams` : 페이지 프로퍼티를 사용하면 요청 시 페이지가 동적으로 전환된다.
  {% endhint %}

<br />

**✅ Streaming**

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fsequential-parallel-data-fetching.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk" alt=""><figcaption></figcaption></figure>

- 스트리밍을 사용하면 서버에서 UI를 점진적으로 렌더링할 수 있다.
- 작업은 청크로 분할되어 준비되는대로 클라이언트로 스트리밍된다. 이를 통해 사용자는 전체 컨텐츠의 렌더링이 완료되기 전에 페이지의 일부를 볼 수 있다.

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fserver-rendering-with-streaming.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk" alt=""><figcaption></figcaption></figure>

- 스트리밍은 기본적으로 Next.js 앱 라우터에 내장되어 있음.
- 제품 페이지의 리뷰가 있다고 가정했을 때, `loading.js`와 UI 컴포넌트를 사용하여 경로 세그먼트 스트리밍을 시작할 수 있다.

---

### Client Components

**🔥 Benefits of Client Rendering**

1. 인터랙티브

- 상태, 효과 및 이벤트 리스너를 사용할 수 있으므로, 사용자에게 즉각적인 피드백을 제공하고 UI를 업데이트할 수 있다.

2. 브라우저 API

- 클라이언트 컴포넌트는 geolocation 또는 localStorage와 같은 브라우저 API에 접근할 수 있으므로 특정 사용 사례에 맞는 UI를 구축할 수 있다.

<br />

**🔥 Using Client Components in Next.js**

클라이언트 컴포넌트를 사용하기 위해선 `"use client"`를 사용한다.

`"use client"` 👉🏻 서버와 클라이언트 컴포넌트 모듈 사이의 경계를 선언하는데 사용

- 즉, 파일에 `"use client"`를 정의하면, 자식 컴포넌트를 포함해 파일로 가져온 다른 모든 모듈이 클라이언트 번들의 일부로 간주된다.

```tsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

중첩된 컴포넌트가 있는 경우, toggle.js에서 onClick과 useState를 사용하면 `"use client"` 지시어가 정의되지 않은 경우 오류가 발생한다. `toggle.js`에 `"use client"` 지시어를 정의하면 REST API를 사용할 수 있는 클라이언트에서 컴포넌트와 그 자식들을 렌더링하도록 지시할 수 있다.

 <figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fuse-client-directive.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk" alt=""><figcaption></figcaption></figure>

<br />

**🔥 How are Client Components Rendered?**
Next.js에서 클라이언트 컴포넌트는 요청이 전체 페이지 로드의 일부인지 또는 후속 탐핵의 일부인지에 따란 다르게 렌더링된다.
