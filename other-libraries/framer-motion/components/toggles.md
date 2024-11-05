---
description: 토글 슬라이딩 애니메이션 구현하기
---

# Toggles

<figure><img src="../../../.gitbook/assets/스크린샷 2024-11-05 오후 2.14.40.png" alt=""><figcaption><p>dark mode</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/스크린샷 2024-11-05 오후 2.14.44.png" alt=""><figcaption><p>light mode</p></figcaption></figure>



<figure><img src="../../../.gitbook/assets/스크린샷 2024-11-05 오후 2.19.42.png" alt=""><figcaption></figcaption></figure>

#### _<mark style="background-color:purple;">⛏️ 토글이 슬라이딩하듯 자연스럽게 이동하려면</mark>_

* `motion.span` 으로 움직일 배경요소를 따로 만들고, 아이콘과 텍스트는 `poistion:relative`, `z-index:10` 으로 띄워놓아야 한다.
* motion.span의 크기는 w-1/2



코드로 구현하면 다음과 같다.

```tsx
function SliderToggle() {
  <div className='relative flex items-center gap-2 text-sm'>
    // 2개의 버튼 요소 
    <button>
      <FiSun className='relative z-10 text-lg md:text-sm' />
      <span className='relative z-10'>Light</span>
    </button>
    <button>
      <FiMoon className='relative z-10 text-lg md:text-sm' />
      <span className='relative z-10'>Dark</span>
    </button>
    // 버튼 아래에서 이동할 배경 요소
    <div>
      <motion.span
        layout
        transitaion={{ type:'spring', damping:15, stiffness: 250}}
        className='h-full w-1/2 rounded-full bg-gradient-to-r from-violet-600 to-indigo-600'
      ></motion.span>
    </div>
  </div>
}
```

* layout 속성은 레이아웃 애니메이션을 활성화시키는 역할
* damping: 반대하는 힘의 세기, 0으로 설정하면 무한으로 진동한다.&#x20;
* stiffness: 스프링의 강도, 값이 높을 수록 갑작스러운 움직임이 더 많이 발생한다.&#x20;
