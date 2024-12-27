# absolute 요소가 클릭영역을 방해할때 👉🏻 pointer-events

```tsx
<div className="relative">
  <button>stage 2</button>
  <span
    className="absolute bottom-[-16px] right-[-16px] size-[65px] bg-contain bg-center pointer-events-none"
    style={{
      backgroundImage: `url(${pointerImageUrl})`,
    }}
  />
</div>
```

<figure><img src="../.gitbook/assets/스크린샷 2024-12-27 오후 4.53.25.png" alt=""><figcaption></figcaption></figure>





stage2의 버튼을 클릭해도 포인터 영역만큼 클릭이 되지 않는 문제가 발생한다. 이때 포인터 영역의 클릭 이벤트를 무시하려면 포인터 요소의 pointer-events CSS 속성을 지정하면 된다.&#x20;



### pointer-events

이 속성은 요소가 포인터 이벤트의 대상이 될 수 있는지 여부를 지정한다.

```css
/* 키워드 값 */
pointer-events: auto;
pointer-events: none;
pointer-events: visiblePainted; /* SVG only */
pointer-events: visibleFill;    /* SVG only */
pointer-events: visibleStroke;  /* SVG only */
pointer-events: visible;        /* SVG only */
pointer-events: painted;        /* SVG only */
pointer-events: fill;           /* SVG only */
pointer-events: stroke;         /* SVG only */
pointer-events: all;            /* SVG only */
/* 전역 값 */
pointer-events: inherit;
pointer-events: initial;
pointer-events: unset;
```



