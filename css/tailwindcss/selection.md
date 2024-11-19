# 선택된 텍스트의 스타일 처리 👉🏻 selection

### `selection`

사용자가 요소 내의 텍스트를 드래그해서 선택할 때 선택된 텍스트의 스타일을 정의하는 가상요소



드래그로 선택된 텍스트의 배경색을 변경하고 싶다면, selection을 이용해 코드를 작성한다.&#x20;

```jsx
<div className='text-zinc-200 selection:bg-zinc-600'></div>
```

* `selection:bg-zinc-600`는 사용자가 요소 내의 텍스트를 드래그해서 선택할 때 선택된 텍스트의 배경색을 `bg-zinc-600` 색상으로 설정
