# NextAuth로 카카오 로그인 구현하기

### install Next Auth

```bash
pnpm add next-auth
```



### add API route

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

_<mark style="background-color:orange;">**REFERENCE**</mark>_

➡️ [NextAuth.js](https://next-auth.js.org/getting-started/example)

***

### NextAuth Type Error

{% hint style="danger" %}
_**ERROR: "authOptions" is not a valid Route export field.**_\
\
app/api/auth/\[...nextauth]/route.ts Type error: Route "app/api/auth/\[...nextauth]/route.ts" does not match the required types of a Next.js Route.

\
👉🏻 authOptions가 모듈로 분리되지 않았을 때 발생
{% endhint %}



<mark style="background-color:yellow;">**기존 파일**</mark>

```javascript
import { PrismaAdapter } from "@auth/prisma-adapter";
import { PrismaClient } from "@prisma/client";
import NextAuth, { NextAuthOptions } from "next-auth";
import KakaoProvider from "next-auth/providers/kakao";

const prisma = new PrismaClient();
export const authOptions: NextAuthOptions = {
  adapter: PrismaAdapter(prisma) as any,
  providers: [
    KakaoProvider({
      clientId: process.env.KAKAO_CLIENT_ID as string,
      clientSecret: process.env.KAKAO_CLIENT_SECRET as string,
    }),
  ],
  secret: process.env.AUTH_SECRET,
  session: {
    strategy: "jwt",
    maxAge: 30 * 24 * 60 * 60, //30일
  },
  callbacks: {
    async signIn({ user, account, profile, email, credentials }) {
      console.log(
        "next-auth signIn",
        user,
        account,
        profile,
        email,
        credentials
      );
      return true;
    },
    async redirect({ url, baseUrl }) {
      if (url.endsWith("/goodbye")) {
        // 로그아웃 처리
        return baseUrl + "/console";
      } else {
        // 로그인 후 또는 기타 리다이렉트 처리
        return baseUrl + "/console/dashboard";
      }
    },
    async session({ session, user, token }) {
      if (session?.user) {
        session.user.id = token.sub;
      }
      return session;
    },
    async jwt({ token, user, account, profile, isNewUser }) {
      return token;
    },
  },
};
const handler = NextAuth(authOptions);

export { handler as GET, handler as POST };

```



<mark style="background-color:yellow;">**해결**</mark>

authOptions를 `@/utils/authOptions.ts` 파일로 분리

```typescript
import { NextAuthOptions } from 'next-auth';
import { PrismaAdapter } from '@auth/prisma-adapter';
import KakaoProvider from 'next-auth/providers/kakao';
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export const authOptions: NextAuthOptions = {
  adapter: PrismaAdapter(prisma) as any,
  providers: [
    KakaoProvider({
      clientId: process.env.KAKAO_CLIENT_ID as string,
      clientSecret: process.env.KAKAO_CLIENT_SECRET as string,
    }),
  ],
  secret: process.env.AUTH_SECRET,
  session: {
    strategy: 'jwt',
    maxAge: 30 * 24 * 60 * 60, //30일
  },
  callbacks: {
    async signIn({ user, account, profile, email, credentials }) {
      console.log(
        'next-auth signIn',
        user,
        account,
        profile,
        email,
        credentials
      );
      return true;
    },
    async redirect({ url, baseUrl }) {
      if (url.endsWith('/goodbye')) {
        // 로그아웃 처리
        return baseUrl + '/console';
      } else {
        // 로그인 후 또는 기타 리다이렉트 처리
        return baseUrl + '/console/dashboard';
      }
    },
    async session({ session, user, token }) {
      if (session?.user) {
        session.user.id = token.sub;
      }
      return session;
    },
    async jwt({ token, user, account, profile, isNewUser }) {
      return token;
    },
  },
};
```



`app/api/auth/[...nextauth]/route.ts` 에서는 authOptions import

```typescript
import NextAuth from 'next-auth';
import { authOptions } from '@/lib/authOptions';

const handler = NextAuth(authOptions);

export { handler as GET, handler as POST };
```



_<mark style="background-color:orange;">**REFERENCE**</mark>_

➡️ [stackoverflow](https://stackoverflow.com/questions/76388994/next-js-13-4-and-nextauth-type-error-authoptions-is-not-assignable-to-type-n)
