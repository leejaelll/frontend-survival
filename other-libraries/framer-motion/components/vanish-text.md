---
description: 일정한 시간마다 텍스트가 사라지고 나타나는 애니메이션 구현하기
---

# Vanish Text

```tsx
const phrases = ['안녕하세요', '반가워요', '잘있어요', '다시만나요']
```

phrase에  담긴 문구를 3초에 하나씩 보여주도록 구현하려고 한다.&#x20;



phrase를 map 함수를 이용하여 순차적으로 렌더링한다. initial은 `opacity:0, scale:0`으로 설정한다.&#x20;

### 🤔 어떤 문구가 보여져야하는지 확인하려면 어떻게 해야할까?

1초에 하나씩 보여준다고 가정하면 1초마다 활성화되는 텍스트가 무엇인지 알아야 한다. active 상태를 만들고, `useEffect`와 `setInterval`을 이용해 3초마다 active의 값을 변경한다.&#x20;

active는 phrase 배열의 index로 사용되어야하기 때문에 `phrase의 길이 - 1`  의 값을 넘어서면 안된다.&#x20;

```tsx
useEffect(() => {
  const intervalRef = setInterval(()=>{
    setActive((prev) => (prev + 1) % phrase.length;
  }, 3000)
  
  return () => clearInterval(intervalRef);
}, [phrases])
```



phrases.map을 실행시킬 때 phrases\[active]가 phrase와 같을 경우에만 `opacity:1, scale:1` 로 적용시킨다.&#x20;

```tsx
variants={{
  active: {
    opacity: 1,
    scale: 1,
  },
  inactive: {
    opacity: 0,
    scale: 0,
  }
}}
```

isActive가 true이면 variants의 active 실행, false이면 variants의 inactive를 실행한다.&#x20;

`const isActive = phrases[active] === phrase`

`animate={isActive ? 'active' : 'inactive'}`

```tsx
<div className='relative mb-14 mt-2 w-full'>
  {phrases.map((phrase) => {
    const isActive = phrases[active] === phrase;

    return (
      <motion.div
        initial={false}
        animate={isActive ? 'active' : 'inactive'}
        className='absolute left-1/2 top-0 w-full text-violet-50'
        key={phrase}
        style={{
          x: '-50%',
        }}
        variants={{
          active: {
            opacity: 1,
            scale: 1,
          },
          inactive: {
            opacity: 0,
            scale: 0,
          },
        }}
      >
        {phrase}
      </motion.div>
    );
  })}
</div>
```
