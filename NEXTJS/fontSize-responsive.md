# 텍스트 길이에 따른 fontSize 조절하는 방법

하나의 페이지를 40개의 번역된 페이지를 만들어야 할 때, ‘Go to Certification’이라는 텍스트가 적힌 버튼이 있다고 가정해보자. 영어는 10자이지만, 카메룬 언어로는 'Accéder à la Certification’로 번역되어 26자가 된다. 즉, 같은 버튼이어도 받아오는 데이터의 텍스트 길이는 얼마나 길지, 얼마나 짧을지 알 수 없다는 것이다.

<br />
이 때, 텍스트가 컨테이너의 width보다 길어져 두 줄로 넘어가는 경우, 텍스트의 fontSize를 줄여서 한 줄로 만들어 달라는 요청이 들어왔다.

```tsx
<div class='mx-auto w-96 space-y-4'>
  <div class='flex items-center justify-center bg-slate-400 text-center rounded-full'>
    <div class='w-[45%] bg-amber-200'>
      <span class='block py-4'>Go to Certification</span>
    </div>
  </div>
  <div class='flex items-center justify-center bg-slate-400 text-center rounded-full'>
    <div class='w-[45%] bg-amber-200'>
      <span class='block py-4'>Accéder à la Certification</span>
    </div>
  </div>
</div>
```

<figure><img src="../../.gitbook/assets/240108-1.png" alt=""><figcaption></figcaption></figure>

### ✅ 구현 로직

1. 노란 박스의 width와 텍스트 박스의 width를 구한다.
2. `텍스트박스 / 노란박스`의 값이 1 이상이면 텍스트가 넘친 상황
3. 텍스트를 얼마나 줄일지에 대한 rate 값을 저장한다. (1 이하면 비율을 1로 고정)
4. fontSize를 적용할 때 rate값을 곱해준다.

```tsx
const [fontSizeRate, setFontSizeRate] = useState(1);
const textContainerRef = useRef();
const textRef = useRef();

useEffect(() => {
  const textContainer = textContainerRef.current.offsetWidth;
  const text = textRef.current.offsetWidth;
  const rate = text / textContainer;

  if (rate <= 1) {
    setFontSizeRate(1);
  } else {
    setFontSizeRate((1 / rate) * 0.8);
  }
}, []);
```

```tsx
<div
  className={classNames(
    'w-[80%] flex justify-center items-center',
    isWindow && 'pt-[1.9%]'
  )}
  style={{ height: mainTextBase * 0.7 }}
  ref={textContainerRef}
>
  <span
    ref={textRef}
    className={classNames('whitespace-nowrap')}
    style={{ fontSize: mainTextBase * fontSizeRate * 0.6 }}
  >
    {page.b_sub4_title}
  </span>
</div>
```

### ✅ 결과

같은 viewport에서 다른 fontSize로 렌더링되는 것을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/240108-2.png" alt=""><figcaption></figcaption></figure>
<figure><img src="../../.gitbook/assets/240108-3.png" alt=""><figcaption></figcaption></figure>
