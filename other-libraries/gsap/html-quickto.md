# 마우스 이동에 따라 HTML 요소 움직이기 👉🏻 quickTo()

```typescript
import gsap from 'gsap';
import { useEffect, useRef } from 'react';

export default function Modal() {
  const modalContainer = useRef(null);
  
  useEffect(() => {
    let xMoveContainer = gsap.quickTo(modalContainer.current, 'left', { duration: 0.8, ease: 'power3' });
    let yMoveContainer = gsap.quickTo(modalContainer.current, 'top', { duration: 0.8, ease: 'power3' });
    
    window.addEventListener('mousemove', (e) => {
      const { pageX, pageY } = e;
      
      xMoveContainer(pageX);
      yMoveContainer(pageY);
    })
  },[])
  
  return (
    <motion.div
      className='h-[350px] w-[400px] absolute bg-white overflow-hidden pointer-events-none flex items-center justify-center'
      ref={modalContainer}
    >
      <div className='absolute h-full w-full' style={{ top: index * -100 + '%', transition: 'top 0.5s cubic-bezier(0.76, 0, 0.24, 1)' }}>
        {projects.map((project, index) => {
          const { src, color } = project;
          return (
            <div className='w-full h-full flex justify-center items-center' key={index} style={{ backgroundColor: color }}>
              <Image src={`/${src}`} width={300} height={0} alt='image' className='h-auto' />
            </div>
          );
        })}
      </div>
    </motion.div>
  );
}
```

### **`gsap.quickTo()`의 역할**

* `quickTo`는 GSAP에서 **반복적으로 같은 속성**을 빠르게 애니메이션할 때 사용
* 이 함수는 **퍼포먼스를 최적화**하며, 상태 변화가 빈번하게 발생하는 **드래그, 마우스 이동** 같은 경우에 유용하다.

#### **`modalContainer.current`**

* 애니메이션을 적용할 DOM 요소(Ref를 통해 참조).

#### **`left`와 `top`**

* #### 애니메이션할 CSS&#x20;

#### **`{ duration: 0.8, ease: 'power3' }`**:

* **`duration`**: 애니메이션의 **지속 시간**(초 단위).
* **`ease: 'power3'`**: 부드러운 가속과 감속 효과를 제공하는 **easing 함수**.

### **`quickTo()`의 반환값**

* `quickTo`는 **함수**를 리턴
  * 이 반환된 함수에 새로운 값을 전달하면 해당 값으로 애니메이션이 즉시 시작
