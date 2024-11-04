# 무한 스크롤 텍스트 구현하기 👉🏻 requestAnimationFrame

<figure><img src="../.gitbook/assets/스크린샷 2024-11-01 오후 6.57.20.png" alt=""><figcaption></figcaption></figure>

```tsx
export default function Home() {
  const firstText = useRef(null);
  const secondText= useRef(null);
  const direction = useRef(-1);
  let xPercent = 0;
  
  return (
    <main className='relative h-svh mb-[100vh] overflow-hidden'>
      <Image src={Background} className='object-cover' fill alt='' priority />
      {/* slider container */}
      <div className='absolute top-[calc(100svh-350px)]'>
        {/* slider */}
        <div ref={slider} className='relative whitespace-nowrap'>
          <p ref={firstText} className='relative m-0 text-white text-[230px] font-medium pr-[50px]'>
            Freelance Developer -
          </p>
          <p ref={secondText} className='m-0 text-white text-[230px] font-medium pr-[50px] last:absolute last:left-full last:top-0'>
            Freelance Developer -
          </p>
        </div>
      </div>
    </main>
  );
}
```

* `Image` fill 속성은 `poistion:absolute`를 가지게 된다.&#x20;
* 텍스트를 이미지 밑으로 내리기 위해 top의 위치를 `[calc(100svh-350px)]`로 적용한다.



### requestAnimateFrame

```tsx
useEffect(() => {
  requestAnimateFrame(animate)
}, [])

const animate = () => {
  if (xPercent < -100) {
    xPercent = 0;
  } else if (xPercent > 0) {
    xPercent = -100;
  }
  gsap.set(firstText.current, { xPercent: xPercent });
  gsap.set(secondText.current, { xPercent: xPercent });
  requestAnimateFrame(animate)
  xPercent += 0.1 * direction.current;
}
```

* `requestAnimationFrame`은 초당 60프레임(fps)로 실행된다. (1fps 당 0.1씩 증가)
* `gsap.set()` 메서드를 통해 x축의 위치를 설정한다.&#x20;

```tsx
if (xPercent < -100) {
  xPercent = 0;
} else if (xPercent > 0) {
  xPercent = -100;
}
```

* 텍스트가 연속적으로 움직이면서 화면의 한쪽 끝에서 사라지더라도 자연스럽게 다시 나타나도록 만드는 무한 스크롤(또는 무한 루프) 효과를 구현
