# 클래스 이름을 동적으로 적용하는 방법

다양한 조건에 따라 클래스 이름을 동적으로 적용하고자 할 때 유용한 유틸리티 함수

```jsx
export function classNames(...classes) {
  return classes.filter(Boolean).join(' ');
}
```

👉🏻 배열의 각 요소를 Boolean 생성자 함수에 전달하여 falsy 값(false, 0, '', null, undefined, NaN)을 걸러낸 후 하나의 문자열로 결합

<br />

### example

```jsx
function Button({ isDisabled }) {
  // 버튼의 클래스를 결정합니다.
  // isDisabled 값에 따라 'disabled' 클래스가 추가되거나 제거됩니다.
  const buttonClass = classNames(
    'btn', // 기본 클래스
    'btn-primary', // 기본 스타일을 위한 클래스
    isDisabled && 'disabled' // 조건부 클래스 (isDisabled가 true일 때만 'disabled' 클래스 추가)
  );

  return (
    <button className={buttonClass} disabled={isDisabled}>
      Click me!
    </button>
  );
}

export default Button;
```

- isDisabled가 true일 때, 버튼의 className은 "btn btn-primary disabled"
- false일 때는 "btn btn-primary"

```jsx
<div
  className={classNames(
    'gap-[5.7%] bg-white rounded-full flex items-center w-full text-black font-medium',
    isWindow ? 'pt-[6%] pb-[4.8%] px-[5.9%]' : 'py-[4.8%] px-[5.9%]',
    isRightAlign && 'flex-row-reverse'
  )}
>
```

---

### tw-merge & clsx dependency를 사용하는 방법

```tsx
import clsx, { type ClassValue } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```
