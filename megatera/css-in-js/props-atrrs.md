# props와 attrs

## 학습 키워드

* props
* attrs

## Props 활용

Styled Component로 만든 컴포넌트도 React 컴포넌트처럼 props를 내려줄 수 있다. ![.](https://s3.ap-northeast-2.amazonaws.com/urclass-images/bWCrU7xKj6obs7trBG6JW-1655885641864.png)

### 1) Props로 조건부 렌더링하기

![](https://s3.ap-northeast-2.amazonaws.com/urclass-images/Wytx1\_owLhXoqInvlFIxs-1655885667230.png)

* 삼항 연산자를 활용해 컴포넌트에 skyblue라는 props가 있는지 확인하고, 있으면 배경색으로 skyblue를, 없을 경우 white를 지정해 주는 코드
* 이 코드에 따라 렌더링 된 컴포넌트 결과 ![](https://s3.ap-northeast-2.amazonaws.com/urclass-images/K0v5btDi6VVO3pNpbtMdk-1655885884007.png)

\


### 2) Props 값으로 렌더링하기

props의 값을 통째로 활용해서 컴포넌트 렌더링에 활용할 수도 있다. ![](https://s3.ap-northeast-2.amazonaws.com/urclass-images/-afhIAw0ZT59\_Alqb5KSi-1655885930269.png)

* 똑같이 삼항 연산자를 사용하고 있지만, 이번에는 props.color가 없다면 white를, props.color가 있다면 props.color의 값을 그대로 가져와서 스타일 속성 값으로 리턴
* 그 결과 color라는 이름으로 받은 props의 값으로 배경색이 지정된다.
