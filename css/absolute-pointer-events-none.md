# absolute 요소가 클릭영역을 방해할때 👉🏻 pointer-events:none

```tsx
<div className="relative">
  <button>Click Button</button>
  <span
    className="absolute bottom-[-16px] right-[-16px] size-[65px] bg-contain bg-center pointer-events-none"
    style={{
      backgroundImage: `url(${pointerImageUrl})`,
    }}
  />
</div>
```

<figure><img src="../.gitbook/assets/스크린샷 2024-12-27 오후 4.53.25.png" alt=""><figcaption></figcaption></figure>

\poi속성은 요소가 포인터 이벤트의 대상이 될 수 있는지 여부를 지정한다.

