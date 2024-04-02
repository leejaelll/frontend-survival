---
cover: >-
  https://images.unsplash.com/photo-1710067958448-9c17cd407df9?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTIwMjY0Nzl8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Schema 정의하기

```prisma
model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}
```

* `@id`는 이 필드가 기본 키임을 나타낸다.&#x20;
* `@default(cuid())` 👉🏻 새로운 세션 레코드가 생성될 때마다 유니크한 식별자를 생성하도록 지시한다.&#x20;
* `CUID(Collision Resistant Unique Indentifier)` 👉🏻 매우 고유한 식별자를 생성하여 세션을 톡특하게 식별한다.



`@relation`

```
user  User  @relation(fields: [userId], references: [id], onDelete: Cascade)
```

* 'User'타입의 필드는 'Session' 모델과 'User' 모델 간의 관계를 정의한다. (Session이 User 모델과 연결되어 있음을 나타낸다.)
* `fields: [userId]` 👉🏻 Session 모델의 userId 필드가 User 모델과의 관계를 매핑하는데 사용한다.&#x20;
* `references: [id]` 👉🏻 Usesr 모델의 id 필드를 참조하고 있음을 의미한다.&#x20;
* `onDelete: Cascade` 👉🏻 User 레코드가 삭제될 때, 해당 User에 연결된 모든 Session 레코드도 함께 삭제되어야 함을 지정한다.&#x20;
