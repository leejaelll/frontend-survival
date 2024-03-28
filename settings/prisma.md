---
cover: >-
  https://images.unsplash.com/photo-1709596046341-579b8918eb87?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTE2MDc2NjR8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Prisma

### Install

```bash
npm i @prisma/client
```



### Setting

`lib/prisma.ts`

```tsx
import { PrismaClient } from '@prisma/client';

const globalForPrisma = global as unknown as { prisma: PrismaClient };

export const prisma = globalForPrisma.prisma || new PrismaClient({});

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```



_<mark style="background-color:green;">**⛏️ prismaClient를 singleton으로 만드는 방법**</mark>_

```tsx
import { PrismaClient } from '@prisma/client';

// singleton instance
const prismaClientSingleton = () => {
  return new PrismaClient();
};

type PrismaClientSingleton = ReturnType<typeof prismaClientSingleton>;

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClientSingleton | undefined;
};

const prisma = globalForPrisma.prisma ?? prismaClientSingleton();

export default prisma;

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```



### Init

```javascript
npx prisma init
```

👉🏻 명령어를 실행하면 prisma 폴더가 생성된다.

*   `prisma/prisma.schema`

    <pre class="language-javascript"><code class="lang-javascript">generator client {
    <strong>  provider = "prisma-client-js"
    </strong>}

    datasource db {
      provider = "postgresql"
      url = env("DATABASE_URL")
    }
    </code></pre>
* `.env` 파일엔 DATABASE\_URL 주소가 생성된다.
  * DATABASE\_URL에는 postgresql에서 사용하는 유저와 비밀번호, 데이터베이스 이름으로 변경한다.
* _**마이그레이션 파일 생성**_
  * Prisma Migrate를 사용해 새로운 마이그레이션을 만든다.
  * `prisma/migrations` 폴더에 새 마이그레이션 파일을 생성한다.
  * `npx prisma migrate dev --name init` 👉🏻 `npx prisma studio`를 실행했을 때, schema에 작성했던 테이블이 보인다면 정상
