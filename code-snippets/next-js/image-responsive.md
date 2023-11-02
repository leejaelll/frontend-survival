# viewport에 따라 이미지 반응형으로 적용하는 방법

> 미디어 쿼리에 따라 이미지의 width, height를 지정하지 않고 viewport width 크기에 따라 이미지가 유동적으로 움직이도록 구현

Next/Image 컴포넌트에 width와 height를 0으로 지정해주고, sizes 속성을 100vw로 만든다.

```tsx
<Image
  src={'/images/main/lump.png'}
  width={0}
  height={0}
  sizes='100vw'
  style={{ height: '100%', width: '100%' }}
  alt='덩어리 이미지'
  className='object-contain'
/>
```

{% hint style="info" %}

### ✅ sizes

- 브라우저가 선택할 이미지의 최적의 크기를 지정하는 데 사용한다.
- 브라우저에게 가능한 이미지 크기의 범위와 각 크기가 필요한 상황을 알려준다.

{% endhint %}

Image 컴포넌트를 감싸고 있는 곳에 이미지가 꽉 찰 수 있도록 영역을 지정해준다. 예시는 div가 flex의 자식요소이기 때문에 `w-full h-full`로 줬다.

```tsx
<div className='w-full h-full'>
  <Image
    src={'/images/main/voice-commentary.png'}
    width={0}
    height={0}
    sizes='100vw'
    style={{ height: '100%', width: '100%' }}
    alt='음성해설 이미지'
    className='object-contain cursor-pointer'
  />
</div>
```
