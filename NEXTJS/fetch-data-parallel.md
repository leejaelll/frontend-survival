# fetchData를 병렬로 가져오는 방법

하나의 컴포넌트에 두 개의 fetchData를 가져와야할 때, await를 각각 작성하게 되면 데이터를 동기적으로 하나씩 불러온 후 컴포넌트를 렌더링한다. 이런 경우 하나의 데이터를 가져오는데 5초가 걸린다고 가정하면 총 10초의 시간이 걸리는 셈이다.

```tsx
async function Page() {
  const movies = await getMoives(id); // 5seconds
  const videos = await getVideos(id); // + 5seconds

  return(
    //...
  )
}
```

<br />

### Promise.all

데이터를 동시에 받아오려면 Promise.all을 이용한다.

```tsx
const [movies, videos] = Promise.all([getMovies(id), getVideos(id)]);
```

👉🏻 Promise.all을 사용하면 두 개의 데이터를 받아오는데 5초의 시간이 소요된다. 하지만 videos 데이터가 5초 이전에 불러와져도 movies의 데이터가 들어오기 전까지는 렌더링을 하지 않는다.

### Suspense

Promise.all의 문제를 해결할 수 있는 방법 👉🏻 **_Suspense_**

두 개의 데이터를 개별적인 컴포넌트로 분리한 후, 외부 컴포넌트에서 가져와서 사용한다.

```tsx
// movie-info.tsx
export default async function MovieInfo({ id }: { id: string }) {
  const movie = await getMovie(id);
  return <h3>{JSON.stringify(movie)}</h3>;
}

// movie-videos.tsx
export default async function MovieVideos({ id }: { id: string }) {
  const videos = await getVideos(id);
  return <h3>{JSON.stringify(videos)}</h3>;
}
```

```tsx
import { Suspense } from 'react';

async function Page({ params: { id } }: { params: { id: string } }) {
  return (
    <div>
      <Suspense>
        <MovieInfo id={id} />
      </Supense>
      <Suspense>
        <MovieVideos id={id} />
      </Supense>
    </div>
  )
}
```

데이터가 다 불러오기 전까지 loading 컴포넌트를 보여주고 싶다면, Suspense fallback에 loading과 관련된 내용을 넣어준다.

```tsx
<Suspense fallback={<h1>Loading...</h1>}>
  <MovieInfo id={id} />
</Supense>
```
