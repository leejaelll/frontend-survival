---
description: gsap 라이브러리를 이용해 애니메이션 만들기
---

# Animations

* [gsap](https://gsap.com/)



```js
import gsap from 'gsap';

gsap.to(mesh.position, { duration: 1, delay: 1, x: 2 });

const tick = () => {
  renderer.render(scene, camera);

  window.requestAnimationFrame(tick);
};

tick();
```
