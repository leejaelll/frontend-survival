# 테두리 애니메이션 효과

```tsx
const turn = useMotionValue(0);

useEffect(() => {
  animate(turn, 1, {
    ease: 'linear',
    duration:5,
    repeat: 'Infinity',
  })
}, [])

const backgroundImage = useMotionTemplate`conic-gradient(from ${turn}turn, #6EE7B700 75%, #6EE7B7 100%)`;
```

{% hint style="info" %}
`useMotionValue`

* useMotionValue는 애니메이션 가능한 값을 생성하는 hook
* turn은 애니메이션 값으로, 처음에는 0으로 설정된다. 애니메이션을 통해 변경되며, 그라디언트의 회전 각도에 영향을 준다.&#x20;
{% endhint %}

`animate(turn, 1, {...})`: turn 값을 0에서 1까지 애니메이션

👉🏻 turn 값은 0에서 1까지 계속해서 선형으로 반복 증가하며, 원뿔형 그라디언트가 계속해서 회전하게 됨



**`conic-gradient(from ${turn}turn, #6EE7B700 75%, #6EE7B7 100%)`**:

* `from ${turn}turn`: 그라디언트의 시작 각도를 `turn` 값에 따라 조정. `turn`은 0에서 1 사이의 값을 가지며, 이는 회전을 나타낸다. `1 turn`은 360도 회전을 의미한다.
* `#6EE7B700 75%`: 그라디언트의 75% 지점까지 투명한 색상.
* `#6EE7B7 100%`: 그라디언트의 100% 지점에서 색상이 `#6EE7B7`으로 변경.

```tsx
<div className='pointer-events-none absolute inset-0 z-10'>
  <motion.div
    style={{
      backgroundImage,
    }}
    className='mask-with-browser-support absolute -inset-[1px] border border-transparent bg-origin-border'
  />
</div>
```



