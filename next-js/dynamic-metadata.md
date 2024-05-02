---
coverY: 0
---

# metadata를 동적으로 적용하기

### generateMetadata

동적 라우팅 경로인 경우, 메타데이터 객체를 반환하는 generateMetadata 함수를 사용할 수 있다.

```tsx
interface IParams {
  params: { id: string };
}

export async function generateMetadata({ params: { id } }: IParams) {
  const movie = await getMovie(id);
  return {
    title: movie.title,
  };
}
```



***

### Static Site Generation (SSG)에서 generateMetadata 사용

```typescript
export async function generateMetadata(): Promise<Metadata> {
  return {
    title: '여기 타이틀',
    description: '여기 부가적인 설명',
    openGraph: {
      images: ['https://...(image 업로드 주소)/hero.png],
    },
    twitter: {
      images: ['https://...(image 업로드 주소)/hero.png],
    },
    icons: {
      icon: require('../favicon.ico').default.src, // ⛏️ favicon을 적용하는 방법
    },
  };
}
```
