# NextJS에서 Google Font를 적용하는 방법

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
