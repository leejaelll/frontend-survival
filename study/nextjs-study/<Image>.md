# <Image>

- 이미지 컴포넌트에 사용할 수 잇는 prop 및 configuration 옵션을 사용하는 방법

```tsx
import Image from 'next/image';

export default function Page() {
  return (
    <Image
      src='/profile.png'
      width={500}
      height={500}
      alt='Picture of the author'
    />
  );
}
```

### Props

| Prop              | Example                                |
| ----------------- | -------------------------------------- |
| src               | src="/profile.png"                     |
| width             | width={500}                            |
| height            | height={500}                           |
| alt               | alt="Picture of the author"            |
| loader            | loader={imageLoader}                   |
| fill              | fill={true}                            |
| sizes             | sizes="(max-width: 768px) 100vw, 33vw" |
| quality           | quality={80}                           |
| priority          | priority={true}                        |
| placeholder       | placeholder="blur"                     |
| style             | style={{objectFit: "contain"}}         |
| onLoadingComplete | onLoadingComplete={img => done()}      |
| onLoad            | onLoad={event => done()}               |
| onError           | onError={event => fail()}              |
| loading           | loading="lazy"                         |
| blurDataURL       | blurDataURL="data:image/jpeg..."       |

---

### Reauired Props

- 이미지 컴포넌트는 `src`, `width`, `height`, `alt` 속성 필수

- `src`

  - 정적으로 가져온 이미지 파일
  - path: string
  - 외부 URL이거나, 내부 경로일 수 있다. (외부 URL을 사용하는 경우, next.config.js의 remotePatterns에 추가해야 함)

- `width` | `height`

  - 렌더링된 너비와 높이를 픽셀 단위로 나타내므로 이미지가 표시되는 크기에 영향을 미친다.
  - 정적으로 가져온 이미지나 `fill` 속성이 있는 이미지를 제외한 경우 필수이다.

- `alt`
  - 스크린리더 및 검색 엔진에 이미지를 설명하는데 사용한다.
  - 이미지가 비활성화 되었거나 이미지를 로드하는 동안에 오류가 발생한 경우의 대체텍스트 역할도 한다.
  - 페이지의 의미를 변경하지 않고 이미지를 대체할 수 있는 텍스트를 포함해야 한다.
  - 이미지를 보완하기 위한 것이 아니며, 이미 제공된 정보를 반복해서는 안된다.
  - 이미지가 장식용이거나 사용자를 위한 것이 아닌 경우, alt는 빈 문자열이어야 한다.

---

### Optional Props

- `loader`

  - 이미지의 URL을 반환하는 함수

  ```tsx
  'use client';

  import Image from 'next/image';

  const imageLoader = ({ src, width, quality }) => {
    return `https://example.com/${src}?w=${width}&q=${quality || 75}`;
  };

  export default function Page() {
    return (
      <Image
        loader={imageLoader}
        src='me.png'
        alt='Picture of the author'
        width={500}
        height={500}
      />
    );
  }
  ```

- `fill`

  - `fill={true}`
  - width, height를 알 수 없을 때 유용하게 사용
  - **_부모 요소는 `position: relative`, `position: absolute` 또는 `position: fixed`여야 한다._**
  - 기본적으로 이미지 요소에는 자동으로 `position: absolute`가 자동으로 할당된다.
  - **이미지에 스타일을 적용하지 않으면 이미지가 컨테이너에 맞게 늘어난다.** 👉🏻 `object-fit: contain`을 설정하면 컨테이너에 맞으면서 가로 세로 비율을 유지할 수 있도록 해준다.
  - `object-fit: cover`를 사용하면 이미지가 전체 컨테이너를 채우고 가로 세로 비율을 유지하기 위해 잘리는 경우가 있는데, 이때 올바르게 보이기 위해선 `overflow: hidden`을 부모 요소에 할당해야 한다.

- `sizes`

  - 미디어 쿼리와 유사한 문자열로, 다양한 중단점에서 **이미지가 얼마나 넓어질지에 대한 정보**를 제공한다.
  - `sizes` 값은 `fill`을 사용하거나 반응형 크기를 갖도록 스타일이 지정된 이미지 성능에 큰 영향을 미친다.
  - `sizes` 속성은 이미지 성능과 관련된 두 가지 중요한 용도로 사용된다.

    1. next/image의 자동 생성된 `srcset`에서 다운로드할 이미지의 크기를 결정하는데 사용된다.

    - 브라우저가 선택할 때 페이지에 있는 이미지의 크기를 아직 알지 못하므로 뷰포트 크기가 같거나 큰 이미지를 선택한다. `sizes` 속성을 사용하면 이미지가 실제로 전체 화면보다 작을 것이라고 브라우저에 알릴 수 있다.
    - `fill` 속성을 사용하여 이미지에 크기 값을 지정하지 않으면 100vw가 사용된다.

    2. `sizes` 속성은 자동으로 생성된 `srcset` 값의 동작을 변경한다.

    - 크기가 지정되지 않은 경우, 고정 크기 이미지 등에 적합한 srcset을 생성한다.
    - 크기가 지정되어 있으면 반응형 이미지에 적합한 큰 srcset이 생성된다.

  - 예를 들어, 모바일 디바이스에서는 전체 너비, 태블릿에서는 2열 레이아웃, 데스크톱 디스플레이에서는 3열 레이아웃으로 표시되는 것을 알고 있다면 srcset 속성을 포함해야 한다.

  ```tsx
  import Image from 'next/image';

  export default function Page() {
    return (
      <div className='grid-element'>
        <Image
          fill
          src='/example.png'
          sizes='(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw'
        />
      </div>
    );
  }
  ```

- `quality`

  - `quality={75}`
  - 최적화된 이미지 품질
  - 1에서 100 사이의 정수이며, 100이 가장 좋은 품질이므로 파일 크기가 가장 크다.
  - 기본값은 75

- `priority`

  - `priority={false}`
  - true이면, 이미지 우선순위가 높은 것으로 간주되며 미리 로드된다.
  - 우선순위를 사용하는 이미지에 대해서는 지연로딩(lazy loading)이 자동으로 비활성화된다.
  - LCP(가장 큰 컨텐츠가 많은 페인트) 요소로 감지된 모든 이미지에 우선순위 속성을 사용해야 한다.
  - 뷰포트 크기에 따라 서로 다른 이미지가 LCP 요소일 수 있으므로, 우선순위가 높은 이미지를 여러 개 사용하는 것이 적절할 수 있다.
  - _<mark style="color:red;">**이미지가 접힌 부분 위에 표시되는 경우에만 사용**</mark>_

- `placeholder`

  - `placeholder = 'empty'`
  - 이미지가 로드되는 동안 사용할 플레이스 홀더
  - 사용 가능한 값은 `blur`, `empty`, `data:image/...`이며, 기본값은 `empty`
  - `blur`인 경우 `blurDataURL` 속성이 placeholder로 사용된다.
    - `src`가 정적 이미지에서 가져온 객체이고, `.jpg`, `.png`, `.webp`, `.avif`인 경우, 이미지가 애니메이션으로 감지되는 경우를 제외하고는 `blurDataURL`이 자동으로 채워진다.
  - 동적 이미지인 경우, `blurDataURL` 속성을 제공해야 한다.
  - `data:image/...`를 사용하면 이미지가 로드되는 동안 데이터 URL이 플레이스홀더로 사용된다.
  - `empty`인 경우, 이미지가 로드되는 동안 플레이스홀더가 표시되지 않고 빈 공간만 표시된다.

---

### Advanced Props

- `style`

  ```tsx
  const imageStyle = {
    borderRadius: '50%',
    border: '1px solid #fff',
  };

  export default function ProfileImage() {
    return <Image src='...' style={imageStyle} />;
  }
  ```

- `onLoad`

  ```tsx
  <Image onLoad={(e) => console.log(e.target.naturalWidth)} />
  ```

  - 이미지가 완전히 로드되고 플레이스홀더가 제거되면 호출되는 콜백 함수
  - 하나의 인수를 사용하여 호출되며, 이 인수는 기본 `<img>` 요소에 대한 참조이다.

- `onError`

  ```tsx
  <Image onError={(e) => console.error(e.target.id)} />
  ```

  - 이미지 로드에 실패할 경우 호출되는 콜백함수
  - onError와 같은 props를 사용하려면 클라이언트 컴포넌트를 사용해 제공된 함수를 직렬화해야한다.

- `loading`
  ```tsx
  loading = 'lazy'; // {lazy} | {eager}
  ```
  - 이미지 로딩동작이며, 기본값은 `lazy`
  - `lazy`를 선택하면 이미지가 뷰포트에서 계산된 거리에 도달할 때까지 이미지 로드를 지연한다.
  - `eager`일 경우, 이미지를 즉시 로드한다.
