# Lottie 사용 방법

[https://lottiefiles.com/](https://lottiefiles.com/)에서 필요한 리소스 다운로드

👉🏻 형식은 json으로 다운받는다.

<br />

```jsx
npm install 'lottie-react'
```

[https://www.npmjs.com/package/lottie-react](https://www.npmjs.com/package/lottie-react)

<br />

### Using the component

```tsx
import React from 'react';
import Lottie from 'lottie-react';
import groovyWalkAnimation from './groovyWalk.json';

const App = () => <Lottie animationData={groovyWalkAnimation} loop={true} />;

export default App;
```

### Using the hook

```tsx
import React from 'react';
import { useLottie } from 'lottie-react';
import groovyWalkAnimation from './groovyWalk.json';

const App = () => {
  const options = {
    animationData: groovyWalkAnimation,
    loop: true,
  };

  const { View } = useLottie(options);

  return <>{View}</>;
};

export default App;
```
