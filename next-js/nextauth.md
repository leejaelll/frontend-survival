# NextAuth로 카카오 로그인 구현하기

### install Next Auth

```bash
pnpm add next-auth
```



### Add API route

`app/api/auth/[...nextauth]/route.ts` 파일 생성 후, providers 만들기

```js
import NextAuth from 'next-auth';
import KakaoProvider from 'next-auth/providers/kakao';

const authOptions = NextAuth({
  providers: [
    // Configure one or more authentication providers
    KakaoProvider({
      clientId: process.env.KAKAO_CLIENT_ID!,
      clientSecret: process.env.KAKAO_CLIENT_SECRET!,
    }),
  ],
  secret: process.env.AUTH_SECRET // 🔥 꼭 추가해줘야 함. (해당 코드가 없을 때 빌드했을 때 서버에러가 발생함)
});

export { authOptions as GET, authOptions as POST };

```



**⛏️ provider 컴포넌트**

`SessionProvider`는 client에서만 동작한다.

layout.tsx에서는 실행할 수 없으므로 따로 provider 컴포넌트를 작성한 후, 하위 컴포넌트를 감싼다.

```tsx
'use client';

import { SessionProvider } from 'next-auth/react';

type SessionProviderProps = {
  children: React.ReactNode;
};

export default function AuthSession({ children }: SessionProviderProps) {
  return <SessionProvider>{children}</SessionProvider>;
}
```

```tsx
import AuthSession from '@/src/components/providers/session-provider';

export default function ConsoleLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <AuthSession>
      <div className='w-screen h-screen mx-auto md:max-w-md lg:max-w-lg xl:max-w-2xl'>
        {children}
      </div>
    </AuthSession>
  );
}
```

***

_<mark style="background-color:orange;">**REFERENCE**</mark>_

➡️ [NextAuth.js](https://next-auth.js.org/getting-started/example)
