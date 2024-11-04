# useScroll

스크롤 애니메이션을 만들때 사용 👉🏻 progress indicators, parallax effects



### Element scroll progress <a href="#element-scroll-progress" id="element-scroll-progress"></a>

```tsx
import "./styles.css";
import { useRef } from "react";
import { motion, useScroll } from "framer-motion";

export default function App() {
  const ref = useRef(null); // 스크롤을 감지할 요소에 적용
  const { scrollXProgress } = useScroll({ container: ref });

  return (
    <>
      <svg id="progress" width="100" height="100" viewBox="0 0 100 100">
        <circle cx="50" cy="50" r="30" pathLength="1" className="bg" />
        <motion.circle
          cx="50"
          cy="50"
          r="30"
          pathLength="1"
          className="indicator"
          style={{ pathLength: scrollXProgress }} // X스크롤의 진행률을 svg 요소에 적용
        />
      </svg>
      <ul ref={ref}> // ➡️ 스크롤바가 생기는 요소, X축으로 스크롤바 생김
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </>
  );
}

```



### Parallax

[https://codesandbox.io/p/sandbox/framer-motion-parallax-i9gwuc?file=%2Fsrc%2FApp.tsx%3A9%2C11\&from-embed](https://codesandbox.io/p/sandbox/framer-motion-parallax-i9gwuc?file=%2Fsrc%2FApp.tsx%3A9%2C11\&from-embed)

```tsx
export default function App() {
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, {
    stiffness: 100,
    damping: 30,
    restDelta: 0.001    
  });
  
  return (
    <>
      {[1, 2, 3, 4, 5].map((image) => (
        <Image id={image} />
      )}
      <motion.div sytle={{ scaleX }}>
    </>
  )
}
```

#### Image Component

```tsx
function Image ({ id }:{ id: number }) {
  const ref = useRef(null);
  const { scrollYProgress } = useScroll({ targe: ref });
  const y = useParallax(scrollYProgress, 300)
  
  return (
    <section>
      <div ref={ref}>
        <img src={`/${id}.jpg`} alt='image' />
      </div>
      <motion.h2 style={{ y }}>{`#00${id}`}</motion.h2>
    </section>
  )
}
```

#### useParallax

```tsx
function useParallax(value: MotionValue<number>, distance: number){
  return useTransform(value, [0, 1], [-distance, distance]);
}
```



### Scroll velocity

```tsx
interface ParallaxProps {
  children: string;
  baseVelocity: number;
}

function ParallaxText({ children, baseVelocity = 100 } : ParallaxProps) {
  const baseX = useMotionValue(0);
  const { scrollY } = useScroll();
  const scrollVelocity = useVelocity(scrollY); // 스크롤의 빠르기를 계산하여 저장 
  const smoothVelocity = useSpring(scrollVelocity, {
    damping: 50,
    stiffness: 400
  });
  const velocityFactor = useTransform(smoothVelocity, [0, 1000], [0, 5], {
    clamp: false
  });
  
 const x = useTransform(baseX, (v) => `${wrap(-20, -45, v)}%`);

 const directionFactor = useRef<number>();
 useAnimationFrame((t, delta) => {
   let moveBy = directionFactor.current * baseVelocity * (delta / 1000);
   if(velocityFactor.get() < 0) {
     directionFactor.current = -1;
   } else if(velocityFactor.get() > 0) {
     directionFactor.current = 1;
   }
   moveBy += directionFactor.current * moveBy * velocityfactor.get();
   baseX.set(baseX.get()+ moveBy);
 }
 
  return (
    <div className="parallax">
      <motion.div className="scroller" style={{ x }}>
        <span>{children} </span>
        <span>{children} </span>
        <span>{children} </span>
        <span>{children} </span>
      </motion.div>
    </div>
  );
}
```

* `scrollY` 값의 변화 속도를 계산하여 `scrollVelocity`에 저장합니다. 이 값은 스크롤의 빠르기를 나타내며, 스크롤이 더 빨리 움직일수록 속도 값이 증가
