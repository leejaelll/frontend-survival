# 스크롤 애니메이션 효과 구현하기 👉🏻 ScrollTrigger

```typescript
import { ScrollTrigger } from 'gsap/all';

useEffect(() => {
  gsap.registerPlugin(ScrollTrigger);
  gsap.to(slider.current, {
    scrollTrigger: {
      trigger: document.documentElement,
      scrub: 0.25,
      start: 0,
      end: window.innerHeight,
      onUpdate: (e) => (direction = e.direction * -1), // direction: 1 || -1
    },
    x: '-500px'
  });
  requestAnimationFrame(animate); // 애니메이션 루프 시작
}, []);
```

* `gsap.registerPlugin(ScrollTrigger)` : 플러그인 등록 &#x20;
* `gsap.to(slider.current, { ... })` : slider ref 요소에 애니메이션을 적용
  * trigger: 애니메이션을 트리거하는 요소, document.documentElement를 지정
  * scrub: 스크롤에 따라 애니메이션을 부드럽게 연결
  * start, end: 애니메이션이 시작하고 끝나는 위치를 정의
  * onUpdate: 스크롤 방향을 업데이트하여 direction을 조절
