# Prisma

### Install

```
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

<br />

**🚧 prismaClient를 singleton으로 만드는 방법**

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

---

### Init

`npx prisma init`

- 명령어를 실행하면 prisma 폴더가 생성된다.

  - `prisma/prisma.schema`

    ```
    generator client {
    provider = "prisma-client-js"
    }

    datasource db {
    provider = "postgresql"
    url = env("DATABASE_URL")
    }
    ```

- `.env` 파일엔 DATABASE_URL 주소가 생성된다.

  - DATABASE_URL에는 postgresql에서 사용하는 유저와 비밀번호, 데이터베이스 이름으로 변경한다.

- **_마이그레이션 파일 생성_**

  - Prisma Migrate를 사용해 새로운 마이그레이션을 만든다.
  - `prisma/migrations` 폴더에 새 마이그레이션 파일을 생성한다.
  - `npx prisma migrate dev --name init`
    👉🏻 `npx prisma studio`를 실행했을 때, schema에 작성했던 테이블이 보인다면 정상
