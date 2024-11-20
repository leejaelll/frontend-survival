---
description: SVG를 이용해 그리드 배경화면 그리기. 근데 이제 그라데이션을 곁들인
cover: ../.gitbook/assets/스크린샷 2024-11-20 오후 3.02.19.png
coverY: 0
---

# 그라데이션 그리드 배경 구현하기

<figure><img src="../.gitbook/assets/스크린샷 2024-11-20 오후 3.09.11.png" alt=""><figcaption></figcaption></figure>

구현 순서는 다음과 같다.&#x20;

1. 그리드 배경화면 만들기
2. 투명에서 색상으로 이어지는 그라데이션 만들기

#### 그리드 배경화면 만들기

svg 요소는 다음과 같다. &#x20;

<div align="left">

<figure><img src="../.gitbook/assets/스크린샷 2024-11-20 오후 4.25.19.png" alt=""><figcaption></figcaption></figure>

</div>

backgroundImage를 사용하면 backgroundRepeat: 'repeat'이 기본값으로 적용되므로 32px의 정사각형 격자무니 형태로 그려지게 된다.

```tsx
<div className="relative overflow-hidden bg-zinc-950">
  <div className="absolute inset-0 z-0">
    <div
      style={{
        backgroundImage: `url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32' width='32' height='32' fill='none' stroke-width='2' stroke='rgb(30 58 138 / 0.5)'%3e%3cpath d='M0 .5H31.5V32'/%3e%3c/svg%3e")`,
      }}
      className='absolute inset-0 z-0'
      />
  </div>
  {/* 그라데이션 컴포넌트 */}
</div>
```



#### 투명 → 색상으로 이어지는 그라데이션 만들기

```tsx
<div className='absolute inset-0 z-10 bg-gradient-to-b from-zinc-950/0 to-zinc-950' />
```
