# Background Image Parallax

➡️ [Live Demo](https://blog.olivierlarose.com/demos/background-image-parallax)

```tsx
import { motion, useScroll, useTransform } from 'framer-motion';
import Image from 'next/image';
import Background from '../../../public/background-2.jpg';

export default function Intro() {
  const { scrollYProgress } = useScroll({
    offset: ['start start', 'end start'],
  }); // 🚧 offset 활용

  // 🚧 스크롤의 진행률(0과 1 사이의 값)은 0vh에서 150vh 사이의 값으로 변환되어 translateY 값으로 사용
  const y = useTransform(scrollYProgress, [0, 1], ['0vh', '150vh']);

  return (
    <div className='h-screen overflow-hidden'>
      <motion.div style={{ y }} className='relative h-full bg-fuchsia-50'>
        <Image src={Background} alt='image' fill className='object-cover' />
      </motion.div>
    </div>
  );
}
```



```tsx
export default function Home() {
  // 🚧 자연스러운 스크롤 효과를 내기 위한 코드
  useEffect(() => {
    const lenis = new Lenis();

    function raf(time: number) {
      lenis.raf(time);
      requestAnimationFrame(raf);
    }

    requestAnimationFrame(raf);
  }, []);
  
  return (
    <main className='relative h-svh '>
      <Intro />
      <Description />
      <Section />
      <div className='h-screen'></div>
    </main>
  );
}
```



```tsx
export default function Section() {
  const container = useRef(null);
  const { scrollYProgress } = useScroll({
    target: container,
    offset: ['start end', 'end start'],
  });
  const y = useTransform(scrollYProgress, [0, 1], ['-10%', '10%']); // translateY

  return (
    <div
      ref={container}
      style={{ clipPath: 'polygon(0% 0, 100% 0%, 100% 100%, 0 100%)' }}
      className='h-screen overflow-hidden relative flex items-center justify-center'
    >
      <div
        className='relative z-10 p-20 mix-blend-difference text-white w-full h-full
       flex flex-col justify-between'
      >
        <p className='uppercase mix-blend-difference w-[50vw] text-[2vw] self-end'>
          Beauty and quality need the right time to be conceived and realised even in a world that is in too much of a hurry.
        </p>
        <p className='text-[5vw] mix-blend-difference uppercase'>Background Parallax</p>
      </div>
      <div className='fixed top-[-10vh] left-0 h-[120vh] w-full'>
        <motion.div className='relative h-full w-full' style={{ y }}>
          <Image src={Background} alt='' fill className='object-cover' />
        </motion.div>
      </div>
    </div>
  );
}

```
