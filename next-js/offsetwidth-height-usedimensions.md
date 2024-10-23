# offsetWidth와 Height를 감지하는 커스텀훅 만들기 👉🏻 useDimensions

```typescript
import { useEffect, useRef } from 'react';

export const useDimensions = (ref: React.RefObject<HTMLElement>) => {
  const dimensions = useRef({ width: 0, height: 0 });

  useEffect(() => {
    if (!ref.current) return;

    dimensions.current.width = ref.current.offsetWidth;
    dimensions.current.height = ref.current.offsetHeight;
  }, []);

  return dimensions.current;
};
```



```typescript
const { height } = useDimensions(containerRef);


return (
  <motion.nav custom={height} ref={containerRef} initial={false} animate={isOpen ? 'open' : 'closed'}>
  //...
)

```

