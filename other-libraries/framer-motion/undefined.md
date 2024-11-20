# 스크롤 방향에 따라 움직이는 텍스트, 스크롤 강도에 따라 기울기 적용하기

스크롤에 따라 텍스트가 움직이는 효과를 주면서 스크롤 강도에 따라 텍스트의 기울기가 변화하는 효과를 구현하고 싶었다.&#x20;

<figure><img src="../../.gitbook/assets/스크린샷 2024-11-19 오후 7.00.38.png" alt=""><figcaption><p> </p></figcaption></figure>

<figure><img src="../../.gitbook/assets/스크린샷 2024-11-19 오후 6.55.48.png" alt=""><figcaption></figcaption></figure>



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



#### 스크롤이 얼만큼 진행되었는지 알아야한다. 👉🏻 `useScroll`

```tsx
const { scrollYProgress } = useScroll()
```

target 속성을 통해 특정 요소에 스크롤 애니메이션을 제한할 수 있다.&#x20;

useRef를 사용하여 참조된 DOM 요소를 지정한다.&#x20;

```tsx
const targetRef = useRef(null)
const { scrollYProgress } = useScroll({ 
  target:targetRef, 
  offset:['start start', 'end start']
})
```

{% hint style="info" %}
offset은 스크롤 애니메이션이 언제 시작되고 끝날지를 정의한다.&#x20;

`['start start', 'end start']` : 요소의 시작부분이 뷰포트의 시작부분과 일치할 때, 요소의 끝 부분이 뷰포트의 시작 부분과 일치할 때를 의미한다.&#x20;
{% endhint %}

<pre class="language-tsx"><code class="lang-tsx"><a data-footnote-ref href="#user-content-fn-1">&#x3C;section className="h-[500vh]" ref={targetRef}></a>
  &#x3C;div className="overflow-hidden h-screen sticky top-0">
    &#x3C;p className="h-screen origin-bottom-left whitespace-nowrap uppercase">
      {`Nothing in this world can take the place of persistence. Talent will not; nothing is more common than unsuccessful men with talent. Genius
        will not; unrewarded genius is almost a proverb. Education will not; the world is full of educated derelicts. Persistence and determination
        alone are omnipotent. The slogan 'Press On!' has solved and always will solve the problems of the human race.
      `}
    &#x3C;/p>
  &#x3C;/div>
&#x3C;/section>
</code></pre>



#### `scrollYProgress` 값의 변화 속도를 계산해야한다. 👉🏻 `useVelocity`

{% hint style="info" %}
useVelocity는 주어진 valeu의 변화율을 추적하고 그에 따라 속도를 계산한다.&#x20;

`const velocityValue = useVelocity(value)`

* 입력 값: `value`는 보통 `motion` 값(예: `scrollYProgress`)이며, `useVelocity`는 해당 값이 얼마나 빨리 변하고 있는지를 측정한다.&#x20;
* 출력 값: `useVelocity`는 `value`의 변화 속도를 나타내는 값(속도 값)을 반환한다. 이 값을 사용해 스크롤 속도에 따른 애니메이션 효과를 만들 수 있다.

`✅`스크롤 속도에 따라 기울기, 크기 변화, 색상 변경 등의 효과를 줄 때 활용할 수 있다.&#x20;
{% endhint %}

```tsx
const scrollVelocity = useVelocity(scrollYProgress);
```



#### 이동한 스크롤만큼 텍스트의 위치를 이동시켜야 한다. 👉🏻 useTransform

```tsx
const xRaw = useTransform(scrollYProgress, [0, 1], [0, -3000]);
const x = useSpring(xRaw, { mass: 3, stiffness: 400, damping: 50 });
```



#### 스크롤 속도에 따라 텍스트의 기울기를 변화시켜야 한다.&#x20;

```tsx
const skewXRaw = useTransform(scrollVelocity, [-1, 1], ['45deg', '-45deg']);
const skewX = useSpring(skewXRaw, { mass: 3, stiffness: 400, damping: 50 });
```



#### skewX, x 값을 텍스트 DOM 요소 스타일에 적용시켜야한다.&#x20;

```tsx
<motion.p 
  style={{ skewX, x }}
  className="h-screen origin-bottom-left whitespace-nowrap uppercase">
  {`Nothing in this world can take the place of persistence. Talent will not; nothing is more common than unsuccessful men with talent. Genius
    will not; unrewarded genius is almost a proverb. Education will not; the world is full of educated derelicts. Persistence and determination
    alone are omnipotent. The slogan 'Press On!' has solved and always will solve the problems of the human race.
  `}
</motion.p>
```

[^1]: 
