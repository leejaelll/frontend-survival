# 문단 좌우 균등배분 정렬 방법

### `text-align`

: 문단 정렬 방식을 정하는 속성

```bash
text-align: left | right | center | justify | initial | inherit;
```

- 문단 양쪽 정렬: `justify`

- 양쪽 모두를 가지런하게 맞추기 위해서 띄어쓰기 간격이 조금 달라진다.

- 영어를 사용할 경우에는 단어갈 길면 간격이 이상하게 보일 수도 있다. 이때 `word-break` 속성으로 조절한다.

<br />

### `word break`

```bash
word-break: normal | break-all | keep-all | initial | inherit;
```

- normal : CJK 문자는 글자 기준으로, CJK 이외의 문자는 단어 기준으로 줄바꿈합니다.
- break-all : 글자 기준으로 줄바꿈합니다.
- keep-all : 단어 기준으로 줄바꿈합니다.

### `in TailwindCSS`

```bash
<div class="text-justify break-all">
  <!-- Your content here -->
</div>
```
