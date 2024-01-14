# DATABASE SETUP

## Prisma

: 타입스크립트 코드와 데이터베이스 사이에 다리 역할

✅ **To Do List**

- [ ] Prisma extension install
- [ ] npm i -D Prisma

_👉🏻 Prisma 사용할 때는 `npx prisma` 로 실행_

`npx prisma init`

👉🏻 prisma 폴더와 .env 파일이 생성됨

<br />

1. DATABASE_URL에 postgresql url이 작성되어있으나, 다른 데이터베이스를 사용한다면 알맞은 DATABASE_URL을 넣어줘야한다.
2. schema.prisma 파일에서 datasource의 provider 설정하기 (proivder는 우리가 사용할 데이터베이스)

   ```tsx
   // This is your Prisma schema file,
   // learn more about it in the docs: https://pris.ly/d/prisma-schema

   generator client {
     provider = "prisma-client-js"
   }

   datasource db {
     **provider = "mysql"**
     url      = env("DATABASE_URL")
   }
   ```

3. 데이터베이스에 사용할 model 만들기

   ```tsx
   model User {
     id        Int      @id @default(autoincrement())
     phone     Int?     @unique
     email     String?  @unique
     name      String
     avatar    String?
     createdAt DateTime @default(now())
     updatedAt DateTime @updatedAt
   }
   ```

   `@id` : model의 id라는 것을 알려주는 역할

   `@default(autoincrement())`: 자동으로 증가

   `Int?`: 옵션으로 만들고 싶다면 ?를 붙인다.

   `@unique`: 유일해야할 때 사용

   <br />

---

## PlanetScale

**PlanetScale**

- https://planetscale.com/
- Serverless 데이터베이스 플랫폼
- severless는 서버가 없다는게 아니라, 서버를 관리하고 유지보수할 필요가 없다는 것을 의미한다.

**Vitess**

- https://vitess.io/
- Vitess는 MySQL을 스케일링하기 위한 데이터베이스 클러스터링 시스템
- 인터넷에서 가장 큰 사이트를 호스팅하는 강력한 오픈 소스 기술

<br />

## Connecting to PlanetScale

🔨 **Install Planet Scale cli**

- `brew install planetscale/tap/pscale`
- `brew install mysql-client` (mysql client설치)
- `brew upgrade pscale` (최신 버전 업데이트)

`pscale auth login`

- 터미널에서 pscale에 로그인

confirm code가 맞는지 확인하기

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/fca2574c-73c9-4a86-9d7b-2fbeff533847/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

<br />

`pscale region list`

- pscale region을 확인할 수 있는 명령어

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/a447591b-5e48-4923-8574-e22525b9cfea/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

<br />

`pscale database create <database> --region <region name>`

- 데이터베이스 생성
- pscale region list에서 확인한 region으로 사용한다.

```tsx
pscale database create carrot-market --region gcp-asia-northeast3
```

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/d77b4357-f660-48da-9d97-ee093011335e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

planetscale overview를 확인해보면 carrot-market 이름의 데이터베이스가 생성된 것을 확인할 수 있다.

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/00d66837-2a0c-4d61-8b58-b3a111e9a282/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

데이터베이스 생성이 완료되었다면, 데이터베이스를 Prisma에 연결해야 한다. Prisma는 환경변수로 DATABASE_URL을 찾고있으므로DATABASE_URL을 설정해 줘야 한다.

👉🏻 **DATABASE_URL이 직접적으로 노출되면 안된다.** 그래서 일반적으로는 암호로 만들고, 서버를 두 개 만드는 방법을 사용함. 하지만 planetScale은 일종의 tunnel을 만들어 보안 연결을 만든다.

<br />

⚙️ **암호 없이 컴퓨터와 PlanetScale 사이에 보안 연결을 만드는 방법**

`pscale connect <database>`

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/b7646f96-afad-47bb-b6d5-5955478303d4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

- 127.0.0.1:3306과 같은 URL을 만들어준다.
  ⚠️ *주의해야할 점은 연결을 계속 해놔야 한다는 것!*
- 127.0.0.1:3306 주소 복사해서 .env파일로 이동하기

  ```tsx
  DATABASE_URL = 'mysql://127.0.0.1:3306/carrot-market';
  ```

  👉🏻 pscale과 컴퓨터 사이의 secure tunnel을 완성!

  <br />

---

## \***\*Push To PlanetScale\*\***

⚙️ **model을 planetscale에 push 해보기**

```tsx
model User {
  id        Int      @id @default(autoincrement())
  phone     Int?     @unique
  email     String?  @unique
  name      String
  avatar    String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

{% hint style="danger" %}

### Vitess를 사용할 때 주의해야할 점

Vitess는 데이터를 저장할 때 데이터베이스를 잘게 쪼개서 여러 서버에 분산시키는 데에 특화되어 있다. User.id가 1인 사용자가 댓글을 작성한다고 가정했을 때, Vitess는 이 댓글을 생성할 때 사용자가 존재하는지 확인하지 않는다. 그 이유는 Vitess가 foriegn key contraint를 지원하지 않기 때문이다.

**_가장 좋은 해결방법은 오류를 내서 멈추는 것이다. 👉🏻 데이터베이스가 이 작업을 하지 않으니 Prisma가 이 역할을 하도록 한다._**

{% endhint %}

<br />

1. client에 `previewFeatures` 옵션 설정

   ```tsx
   generator client {
     provider        = "prisma-client-js"
     previewFeatures = ["referentialIntegrity"] // 다른 객체에 연결할 때 그 객체가 존재하길 바라는 것을 의미
   }
   ```

2. db에 previewFeatures 작업은 Prisma가 한다고 전달하기

   ```tsx
   datasource db {
     provider             = "mysql"
     url                  = env("DATABASE_URL")
     referentialIntegrity = "prisma" // referentialIntegrity 작업은 prisma가 한다는 의미
   }
   ```

<br />

⚙️ **Prisma를 이용해서 변경된 내용들을 PlanetScale에 push하는 방법**

💡 잊지 않고 pscale connect가 실행되고 있는지 먼저 확인하기!

`npx prisma db push`

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/bf903d12-d5ae-4b6c-8d62-0953f8d27b2e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

- 먼저 .env 파일에서 환경변수를 불러온다.
- MySQL 데이터베이스를 찾아서 연결한다.
- 데이터베이스가 schema와 잘 동기화되었다는 메세지를 보여준다.
- ✔ Generated Prisma Client 👉🏻 Prisma Client가 생성되었다는 메세지

<br />

---

## Prisma Client

`npx prisma studio`

prisma studio는 schema.prisma를 읽고 데이터를 보여주는 역할을 한다.

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/b805f5e5-74d4-4c8e-bfa0-70f527b413bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

![스크린샷.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9e43fdd6-1e8d-4ef6-9993-8681828a9df4/58e7824a-5069-4177-85a9-a0a103ed8640/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA.png)

👉🏻 User라는 model을 만들었기 때문에 User 데이터 필드를 볼 수 있다.

<br />

**✅ Prisma Client**

- TypeScript 및 Node.js용 직관적인 데이터베이스 클라이언트
- Prisma Client는 생각하는 방식으로 구성하고 앱에 맞춤화된 유형으로 Prisma 스키마에서 자동 생성되는 쿼리 빌더
- mongo DB의 mongoose와 같은 역할

1. prisma install

   ```tsx
   npm install @prisma/client
   ```

2. client 초기화 시키기

   libs > client.ts

   ```tsx
   import { PrismaClient } from '@prisma/client';

   const client = new PrismaClient(); // create client
   ```

   `npx prisma generate` 를 해보면 node_modules에 prisma/client가 생성된 것을 볼 수 있다.

   ✔ Generated Prisma Client (3.8.1 | library) to ./node_modules/@prisma/client in 32ms

   ```tsx
   client.user.create({
     data: {
       email: 'hi',
       name: 'hi',
     },
   });
   ```

   client는 User의 타입을 생성해놓기 때문에 user를 생성할 때 안전하게 생성할 수 있다.

<br />

---

## API Routes

- [ ] pages > api 폴더 만들기
- [ ] client-test.tsx 파일 만들기

💡 **API Routes를 만들기 위한 규칙 👉🏻 `export default function`**

Next.js는 request와 response 객체를 제공한다.

req, res type

- req: NextApiRequest
- res: NextApiResponse

```tsx
import { NextApiRequest, NextApiResponse } from 'next';

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  res.json({
    ok: true,
    data: 'xx',
  });
}
```

`👉🏻 localhost:3000/api/client-test` 로 가보면 res.json으로 전달한 객체를 확인할 수 있다. _(Next JS의 장점!)_
