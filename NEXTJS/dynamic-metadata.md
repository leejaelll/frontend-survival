# metadata를 동적으로 적용하는 방법

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
