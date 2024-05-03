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



#### title을 동적으로 변경하는 방법

페이지를 이동할 때마다 `{title} | Movie` 와 같은 형식으로 title을 보여주고 싶을 때, 모든 컴포넌트마다 Metadata를 만들지 않고, %s를 사용해서 동적으로 title을 변경시킬 수 있다.&#x20;

```typescript
interface IParams {
  params: { id: string };
}

export async function generateMetadata({ params: { id } }: IParams) {
  const movie = await getMovie(id);
  return {
    title: {
      absolute: '',
      default: 'Next.js Tutorial - Movie',
      template: '%s| Movie',
    }
  };
}
```

{% code title="app/blog/page.tsx" %}
```typescript
export default function Blog(){
    return {
       title: 'Blog'
    }
}
```
{% endcode %}

* app/page.tsx에 접근했을 때 title 👉🏻 Next.js Tutorial - Movie
* app/blog/page.tsx에 접근했을 때 title 👉🏻 Blog | Movie

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
