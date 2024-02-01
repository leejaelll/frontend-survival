# NextJS에서 Local Font와 Google Font 적용하기

### 🔥 Local Font 적용

pulic 폴더에 다운받은 폰트를 넣어준다.

<figure><img src="../../.gitbook/assets/231010-1.png" alt=""><figcaption></figcaption></figure>

`global.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --foreground-rgb: 0, 0, 0;
  --background-start-rgb: 214, 219, 220;
  --background-end-rgb: 255, 255, 255;
}

@font-face {
  font-family: 'zer01ne'; // ✅ font-family로 적용할 이름
  src: url('/static/fonts/zer01neotsb.ttf'); // ✅ 폰트가 저장되어있는 위치
}
```

해당 폰트를 사용할 때는 `font-[폰트명]`을 클래스에 입력한다.

```html
<div className="font-[zer01ne-al]">
  <div className="flex items-center flex-col">
    <h1>{title}</h1>
    <button>확인</button>
  </div>
</div>
```

---

### 🔥 Google Font 적용

next/font/google에서 적용하고자 하는 폰트를 import 해온다.

```tsx
import { Noto_Serif_KR } from 'next/font/google';
```

<figure><img src="../../.gitbook/assets/231011-1.png" alt=""><figcaption></figcaption></figure>

사용할 font-weight와 속성들을 지정해준다.

```tsx
const notoSerif = Noto_Serif_KR({
  subsets: ['latin'],
  display: 'swap',
  weight: ['400', '500', '200'],
});
```

`RootLayout`에 className으로 지정해준다.

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang='en'>
      <body className={notoSerif.className}>
        <RecoilRootProvider>{children}</RecoilRootProvider>
      </body>
    </html>
  );
}
```
