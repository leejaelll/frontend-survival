# Authentication

#### 프로세스 세 가지 개념

👉🏻 인증, 세션관리, 권한부여

1. 인증(Authentication)
   * 사용자는 이름과 비밀번호와 같이 자신이 가지고 있는 것으로 신원을 증명해야 한다.
2. 세션 관리(Session Management)
   * 요청 전반에 걸쳐 사용자의 인증 상태를 추적한다.
3. 권한 부여(Authorization)
   * 사용자가 접근할 수 있는 경로와 데이터를 결정한다.

***

## Authentication

* 리액트의 Sever Actions와 `useActionState()`와 함께 form 요소를 사용할 수 있다.
* Server Actions는 항상 서버에서 실행되므로 인증 로직을 처리하기 위한 안전한 환경을 제공함

### Validate form fields on the server

Zod를 사용하여 오류 메세지와 form schema를 정의할 수 있다.

```typescript
import { z } from 'zod'
 
export const SignupFormSchema = z.object({
  name: z
    .string()
    .min(2, { message: 'Name must be at least 2 characters long.' })
    .trim(),
  email: z.string().email({ message: 'Please enter a valid email.' }).trim(),
  password: z
    .string()
    .min(8, { message: 'Be at least 8 characters long' })
    .regex(/[a-zA-Z]/, { message: 'Contain at least one letter.' })
    .regex(/[0-9]/, { message: 'Contain at least one number.' })
    .regex(/[^a-zA-Z0-9]/, {
      message: 'Contain at least one special character.',
    })
    .trim(),
})
 
export type FormState =
  | {
      errors?: {
        name?: string[]
        email?: string[]
        password?: string[]
      }
      message?: string
    }
  | undefined
```



authentication provider의 API 또는 데이터베이스에 대한 불필요한 호출을 방지하기 위해 정의된 스키마와 일치하지 않는 경우, 서버 작업 초기에 반환할 수 있다.

```typescript
import { SignupFormSchema, FormState } from '@/app/lib/definitions'
 
export async function signup(state: FormState, formData: FormData) {
  // Validate form fields
  const validatedFields = SignupFormSchema.safeParse({
    name: formData.get('name'),
    email: formData.get('email'),
    password: formData.get('password'),
  })
 
  // If any form fields are invalid, return early
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }
 
  // Call the provider or db to create a user...
}
```



`useActionState()` 훅을 사용해 form이 제출되는 동안 유효성 검사 오류와 보류 중인 상태를 표시할 수 있다.

```tsx
'use client'
 
import { useActionState } from 'react'
import { signup } from '@/app/actions/auth'
 
export function SignupForm() {
  const [state, action, pending] = useActionState(signup, undefined)
 
  return (
    <form action={action}>
      <div>
        <label htmlFor="name">Name</label>
        <input id="name" name="name" placeholder="Name" />
      </div>
      {state?.errors?.name && <p>{state.errors.name}</p>}
 
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" name="email" placeholder="Email" />
      </div>
      {state?.errors?.email && <p>{state.errors.email}</p>}
 
      <div>
        <label htmlFor="password">Password</label>
        <input id="password" name="password" type="password" />
      </div>
      {state?.errors?.password && (
        <div>
          <p>Password must:</p>
          <ul>
            {state.errors.password.map((error) => (
              <li key={error}>- {error}</li>
            ))}
          </ul>
        </div>
      )}
      <button aria-disabled={pending} type="submit">
        {pending ? 'Submitting...' : 'Sign up'}
      </button>
    </form>
  )
}
```



### Create a user or check user credentials

유효성 검사 후, 새 계정을 만들거나 데이터베이스를 호출하여 사용자가 존재하는지 확인할 수 있다.

```tsx
export async function signup(state: FormState, formData: FormData) {
  // 1. Validate form fields
  // ...
 
  // 2. Prepare data for insertion into database
  const { name, email, password } = validatedFields.data
  // e.g. Hash the user's password before storing it
  const hashedPassword = await bcrypt.hash(password, 10)
 
  // 3. Insert the user into the database or call an Auth Library's API
  const data = await db
    .insert(users)
    .values({
      name,
      email,
      password: hashedPassword,
    })
    .returning({ id: users.id })
 
  const user = data[0]
 
  if (!user) {
    return {
      message: 'An error occurred while creating your account.',
    }
  }
 
  // TODO:
  // 4. Create user session
  // 5. Redirect user
}
```

***

## Session Management

#### 세션의 두 가지 개념

1. Stateless
   * 세션 데이터(또는 토큰)는 브라우저의 쿠키에 저장된다.
   * 쿠키는 각 요청과 함께 전송되어 세션을 서버에서 검증할 수 있다.
   * 간단하지만 올바르게 구현하지 않으면 보안성이 떨어질 수 있다.
2. Database
   * 세션 데이터는 데이터베이스에 저장되고, 사용자의 브라우저는 암호화된 세션 ID만 수신한다.
   * 이 방법이 더 안전하지만 복잡하고 서버 리소스를 더 많이 사용할 수 있다.

### Stateless Sessions

🚧 **무상태 세션을 만들고 관리하는 방법**

1. secret key를 만들고 환경변수로 저장한다.
2. 세션 관리 라이브러리를 사용하여 세션 데이터를 암호화/복호화하는 로직을 작성한다.

```typescript
import 'server-only'
import { SignJWT, jwtVerify } from 'jose'
import { SessionPayload } from '@/app/lib/definitions'
 
const secretKey = process.env.SESSION_SECRET
const encodedKey = new TextEncoder().encode(secretKey)
 
export async function encrypt(payload: SessionPayload) {
  return new SignJWT(payload)
    .setProtectedHeader({ alg: 'HS256' })
    .setIssuedAt()
    .setExpirationTime('7d')
    .sign(encodedKey)
}
 
export async function decrypt(session: string | undefined = '') {
  try {
    const { payload } = await jwtVerify(session, encodedKey, {
      algorithms: ['HS256'],
    })
    return payload
  } catch (error) {
    console.log('Failed to verify session')
  }
}
```

3. Next.js `cookies()` API를 사용하여 쿠키를 관리한다.
   * HttpOnly: 클라이언트 측에서 쿠키에 엑세스하는 것을 방지한다.
   * Secure: https를 사용하여 쿠키를 보낸다.
   * SameSite: 쿠키를 사이트 간 요청과 함께 보낼 수 있는지 여부를 지정한다.
   * Max-Age or Expires: 일정 기간이 지난 후에는 쿠키를 삭제한다.
   * Path: 쿠키의 URL 경로를 정의한다.

```typescript
import 'server-only'
import { cookies } from 'next/headers'
 
export async function createSession(userId: string) {
  const expiresAt = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
  const session = await encrypt({ userId, expiresAt })
 
  cookies().set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expiresAt,
    sameSite: 'lax',
    path: '/',
  })
}
```



서버 작업으로 돌아가서 `createSession()`함수를 호출하고 `redirect()` API를 사용하여 사용자를 적절한 페이지로 리디렉션할 수 있다.

```ts
import { createSession } from '@/app/lib/session'
 
export async function signup(state: FormState, formData: FormData) {
  // Previous steps:
  // 1. Validate form fields
  // 2. Prepare data for insertion into database
  // 3. Insert the user into the database or call an Library API
 
  // Current steps:
  // 4. Create user session
  await createSession(user.id)
  // 5. Redirect user
  redirect('/profile')
}
```



🚧 **세션 업데이트 / 삭제**&#x20;

만료 시간을 연장하는 방법

```ts
import 'server-only'
import { cookies } from 'next/headers'
import { decrypt } from '@/app/lib/session'
 
export async function updateSession() {
  const session = cookies().get('session')?.value
  const payload = await decrypt(session)
 
  if (!session || !payload) {
    return null
  }
 
  const expires = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
  cookies().set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expires,
    sameSite: 'lax',
    path: '/',
  })
}
```



세션을 삭제하기 위해선 쿠키를 삭제해야 한다.

```ts
import 'server-only'
import { cookies } from 'next/headers'
 
export function deleteSession() {
  cookies().delete('session')
}
```

deleteSession 함수를 재사용할 수도 있다.

```ts
import { cookies } from 'next/headers'
import { deleteSession } from '@/app/lib/session'
 
export async function logout() {
  deleteSession()
  redirect('/login')
}
```



### Database Sessions

🚧 **데이터베이스 세션을 만들고 관리하는 방법**

1. 세션과 데이터를 저장할 데이터베이스에 테이블을 만든다.
2. 세션을 삽입, 업데이트, 삭제하는 기능을 구현한다.
3. 사용자의 브라우저에 저장하기 전에 세션 ID를 암호화하고, 데이터베이스와 쿠키가 동기화된 상태를 유지하도록 한다.

```ts
import cookies from 'next/headers'
import { db } from '@/app/lib/db'
import { encrypt } from '@/app/lib/session'
 
export async function createSession(id: number) {
  const expiresAt = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
 
  // 1. Create a session in the database
  const data = await db
    .insert(sessions)
    .values({
      userId: id,
      expiresAt,
    })
    // Return the session ID
    .returning({ id: sessions.id })
 
  const sessionId = data[0].id
 
  // 2. Encrypt the session ID
  const session = await encrypt({ sessionId, expiresAt })
 
  // 3. Store the session in cookies for optimistic auth checks
  cookies().set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expiresAt,
    sameSite: 'lax',
    path: '/',
  })
}
```

***

## Authorization

사용자가 인증되고 세션이 생성되면 애플리케이션 내에서 사용자가 액세스하고 수행할 수 있는 작업을 제어하는 authorization을 구현할 수 있다.

#### 권한 확인의 두 가지 유형

1. Optimistic
   * 쿠키에 저장된 세션 데이터를 사용하여 사용자가 경로에 엑세스하거나 작업을 수행할 권한이 있는지 확인
   * UI 요소를 표시/숨김 권한이나 역할이나 사용자를 리디렉션하는 것과 같은 빠른 작업에 유용함
2. Secure
   * 사용자가 데이터베이스에 저장된 세션 데이터를 사용하여 경로에 엑세스하거나 작업을 수행할 권한이 있는지 확인
   * 이러한 검사는 보다 안전하며 민감한 데이터 또는 작업에 엑세스해야하는 작업에 사용됨



### Optimistic checks with Middleware

권한에 따라 미들웨어를 사용하고 사용자를 리디렉션하려는 경우가 있다. 미들웨어는 모든 경로에서 실행되므로 쿠키에서만 세션을 읽고 성능 문제를 방지하기 위해 데이터베이서 검사를 피하는 것이 좋다.

```ts
import { NextRequest, NextResponse } from 'next/server'
import { decrypt } from '@/app/lib/session'
import { cookies } from 'next/headers'
 
// 1. Specify protected and public routes
const protectedRoutes = ['/dashboard']
const publicRoutes = ['/login', '/signup', '/']
 
export default async function middleware(req: NextRequest) {
  // 2. Check if the current route is protected or public
  const path = req.nextUrl.pathname
  const isProtectedRoute = protectedRoutes.includes(path)
  const isPublicRoute = publicRoutes.includes(path)
 
  // 3. Decrypt the session from the cookie
  const cookie = cookies().get('session')?.value
  const session = await decrypt(cookie)
 
  // 5. Redirect to /login if the user is not authenticated
  if (isProtectedRoute && !session?.userId) {
    return NextResponse.redirect(new URL('/login', req.nextUrl))
  }
 
  // 6. Redirect to /dashboard if the user is authenticated
  if (
    isPublicRoute &&
    session?.userId &&
    !req.nextUrl.pathname.startsWith('/dashboard')
  ) {
    return NextResponse.redirect(new URL('/dashboard', req.nextUrl))
  }
 
  return NextResponse.next()
}
 
// Routes Middleware should not run on
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)'],
}
```



### Creating a Data Access Layer (DAL)

DAL에는 사용자가 애플리케이션과 상호작용할 때 사용자의 세션을 확인하는 기능이 포함되어야 한다.

```ts
import 'server-only'
 
import { cookies } from 'next/headers'
import { decrypt } from '@/app/lib/session'
 
export const verifySession = cache(async () => {
  const cookie = cookies().get('session')?.value
  const session = await decrypt(cookie)
 
  if (!session?.userId) {
    redirect('/login')
  }
 
  return { isAuth: true, userId: session.userId }
})
```

데이터 요청, 서버 작업, 경로 핸들러에서 함수를 호출할 수 있다.

```ts
export const getUser = cache(async () => {
  const session = await verifySession()
  if (!session) return null
 
  try {
    const data = await db.query.users.findMany({
      where: eq(users.id, session.userId),
      // Explicitly return the columns you need rather than the whole user object
      columns: {
        id: true,
        name: true,
        email: true,
      },
    })
 
    const user = data[0]
 
    return user
  } catch (error) {
    console.log('Failed to fetch user')
    return null
  }
})
```



### Using Data Transfer Objects (DTO)

데이터를 검색할 때는 애플리케이션에서 사용할 필수 데이터만 반환하고 전체 객체는 반환하지 않는 것이 좋다. 예를 들어, 사용자 데이터를 가져오는 경우 비밀번호, 전화번호 등을 포함할 수 있는 전체 사용자 객체가 아닌 사용자의 ID와 이름만 반환할 수 있다.

```ts
import 'server-only'
import { getUser } from '@/app/lib/dal'
 
function canSeeUsername(viewer: User) {
  return true
}
 
function canSeePhoneNumber(viewer: User, team: string) {
  return viewer.isAdmin || team === viewer.team
}
 
export async function getProfileDTO(slug: string) {
  const data = await db.query.users.findMany({
    where: eq(users.slug, slug),
    // Return specific columns here
  })
  const user = data[0]
 
  const currentUser = await getUser(user.id)
 
  // Or return only what's specific to the query here
  return {
    username: canSeeUsername(currentUser) ? user.username : null,
    phonenumber: canSeePhoneNumber(currentUser, user.team)
      ? user.phonenumber
      : null,
  }
}
```

👉🏻 DAL에서 데이터 요청과 권한 부여 논리를 중앙 집중화하고 DTO를 사용하면 모든 데이터 요청이 안전하고 일관되도록 보장할 수 있다.



### Server Component

서버 컴포넌트에서 인증을 체크하는 것은 역할 기반 액세스에 유용하다. 사용자 역할에 따라서 컴포넌트를 조건부로 렌더링할 때 사용한다.

```tsx
import { verifySession } from '@/app/lib/dal'
 
export default function Dashboard() {
  const session = await verifySession()
  const userRole = session?.user?.role // Assuming 'role' is part of the session object
 
  if (userRole === 'admin') {
    return <AdminDashboard />
  } else if (userRole === 'user') {
    return <UserDashboard />
  } else {
    redirect('/login')
  }
}
```

* `verifySession()`DAL의 함수를 사용하여 'admin', 'user' 및 권한 없는 역할을 확인



### Layouts and checks

레이아웃에서 검사를 수행할 땐 주의하기. 레이웃은 탐색 시 다시 렌더링되지 않으며, 경로가 변경될 때마다 사용자 세션을 검사하지 않는다는 것을 의미한다.

대신 데이터 소스나 조건부로 렌더링될 구성 요소에 가깝게 검사를 수행해야 한다. 예를 들어, 사용자 데이터를 패치하고 nav에 사용자 이미지를 표시하는 공유 레이아웃을 고려할 때 레이아웃에서 인증 검사를 수행하는 대신, `getUser()`에서 사용자 데이터를 패치하고 DAL에서 인증 검사를 수행해야 한다.

이렇게 하면 `getUser()` 애플리케이션 내에서 호출되는 모든 위치에서 인증 확인이 수행되고, 개발자가 사용자에게 데이터 액세스 권한이 있는지 확인하는 것을 잊지 않게 된다.

```tsx
export default async function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  const user = await getUser();
 
  return (
    // ...
  )
}
```

```ts
export const getUser = cache(async () => {
  const session = await verifySession()
  if (!session) return null
 
  // Get user ID from session and fetch data
})
```



### Server Actions

공개 API 엔드포인트와 동일한 보안 고려사항으로 서버 작업을 처리하고 , 사용자가 변형을 수행할 수 있는지 확인한다.

```ts
'use server'
import { verifySession } from '@/app/lib/dal'
 
export async function serverAction(formData: FormData) {
  const session = await verifySession()
  const userRole = session?.user?.role
 
  // Return early if user is not authorized to perform the action
  if (userRole !== 'admin') {
    return null
  }
 
  // Proceed with the action for authorized users
}
```



### Route Handlers

```ts
import { verifySession } from '@/app/lib/dal'
 
export async function GET() {
  // User authentication and role verification
  const session = await verifySession()
 
  // Check if the user is authenticated
  if (!session) {
    // User is not authenticated
    return new Response(null, { status: 401 })
  }
 
  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    // User is authenticated but does not have the right permissions
    return new Response(null, { status: 403 })
  }
 
  // Continue for authorized users
}
```

***

#### Reference

https://nextjs.org/docs/app/building-your-application/authentication
