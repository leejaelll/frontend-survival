# 스크롤 방향에 따라 움직이는 텍스트, 스크롤 강도에 따라 기울기 적용하기

스크롤에 따라 텍스트가 움직이는 효과를 주면서 스크롤 강도에 따라 텍스트의 기울기가 변화하는 효과를 구현하고 싶었다.&#x20;

<figure><img src="../.gitbook/assets/스크린샷 2024-11-19 오후 7.00.38.png" alt=""><figcaption><p> </p></figcaption></figure>

<figure><img src="../.gitbook/assets/스크린샷 2024-11-19 오후 6.55.48.png" alt=""><figcaption></figcaption></figure>



#### 스크롤을 할 만큼의 높이가 필요하다.&#x20;

```tsx
<section className="h-[500vh]"></section>
```

#### 텍스트가  부모 요소의 영역을 넘어가더라도 한 줄로 위치해있어야 한다.

```tsx
<div className="overflow-hidden h-screen sticky top-0">
  <p className="origin-bottom-left whitespace-nowrap uppercase">
    {`Nothing in this world can take the place of persistence. Talent will not; nothing is more common than unsuccessful men with talent. Genius
      will not; unrewarded genius is almost a proverb. Education will not; the world is full of educated derelicts. Persistence and determination
      alone are omnipotent. The slogan 'Press On!' has solved and always will solve the problems of the human race.
    `}
  </p>
</div>
```

```tsx
```
