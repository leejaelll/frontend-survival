# NextJS에서 로컬 폰트를 적용하는 방법

> 다운받은 폰트를 NextJS에서 적용해보자.

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
