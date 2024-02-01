# Testing Library

## 학습 키워드

* Jest
* Describe-Context-It 패턴
* React Testing Library

\


## Jest

🚀 [Jest 공식문서](https://jestjs.io/)

* 페이스북에서 개발한 테스트 프레임워크
* Jest 이전에 가장 인기가 많았던 테스트 도구는 Mocha, Jasmine

{% hint style="info" %}
#### 🙋🏻‍♂️ Jest가 테스트 프레임워크 중 가장 인기가 많은 이유가 뭘까?

* 사용이 쉽고 간편하다.
* Jest는 자동화된 모킹을 제공하여, 별도의 모킹 라이브러리를 사용하지 않아도 된다.
* Jest는 병렬 처리를 지원하므로, 빠른 속도로 테스트를 실행할 수 있다.
{% endhint %}

\


{% hint style="info" %}
#### 🫥 프론트엔드에서 테스트를 해야할까?

프론트엔드에서 다루는 UI는 금방금방 변화하는데 테스트를 하는 게 맞을까? 혹은 테스트를 해야한다면 어떤 것들을 테스트해야할까?

✅ 프론트엔드 테스팅 대상

* 사용자 이벤트 처리
* API 서버 통신

👉🏻 자주 바뀌는 것들은 테스트의 대상이 아니다. UI에 대한 컨트롤러가 테스트의 대상일뿐!
{% endhint %}

테스팅 도구

Mocah와 Chai처럼 RSpc의 decribe-it을 지원하고 expect로 단언(assertion)할 수 있다.

테스트 코드를 작성해본 경험이 없으니 기본적인 테스트 코드가 어떻게 생겼는지 보고 만들어보자.

```jsx
// 테스트 코드 파일 생성
touch src/main.test.ts

// 테스트 실행
npm test
```

```jsx
function add(x: number, y: number): number {
  return x + y;
}

test('Test', () => {
  expect(add(1, 2)).toBe(3);
});
```

BDD 스타일의 코드

```jsx
describe('add', () => {
  it('returns sum of two numbers', () => {
    expect(add(1, 2).toBe(3));
  });

  it('returns number', () => {
    expect(typeof add(1, 2)).toBe('number');
  });
});
```

BDD 스타일의 코드를 작성함으로써 표현력이 좋아지고 생각할 수 있는 기회가 주어진다.

\


## React Testing Library

🚀 [React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro/)

🚀 [jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)

UI 테스트에 특화된 라이브러리 👉🏻 컴포넌트를 테스트한다고 생각하면 된다.

* Greeting 컴포넌트에 props로 전달한 name이 화면에 잘 보이는가?

\


간단한 테스트 코드

```jsx
// Greeting.tsx
export default function Greeting({ name }: { name: string }) {
  return <p>Hello, {name}!</p>;
}

// Greeting.test.tsx
import { render } from '@testing-library/react';

import Greeting from './Greeting';

test('Greeting', () => {
  render(<Greeting name="world" />);
});
```

`screen.getByText` 를 사용해서 텍스트를 가진 엘리먼트를 찾을 수 있다.

```jsx
test('Greeting', () => {
  render(<Greeting name="world" />);

  screen.getByText('Hello, world!');
});
```

* Greeting 함수는 name=”world”를 전달했으니 `‘Hello, world!’` 를 리턴할 것이고 테스트는 정상적으로 통과됨
* 만약 `screen.getByText('Hello, world')` 라고 작성하면? 👉🏻 정확하게 `'Hello, world'` 를 가진 엘리먼트를 찾으려고 함
*   일부만 가지고 찾고 싶다면 어떻게 해야할까? 👉🏻 정규표현식 사용

    ```jsx
    test('Greeting', () => {
      render(<Greeting name="world" />);

      screen.getByText(/Hello/);
    });
    ```
*   `getByText` 는 없으면 에러를 내지만, queryByText는 없을 때 에러를 내지 않음

    ```jsx
    expect(scree.queryByText(/Hi/)).toBeFalsy();

    // 위 코드 보다 좋은 방법
    expect(scree.queryByText(/Hi/)).not.toBeInTheDocument();
    ```

> _React Testing Library는 거의 E2E Test처럼 쓸 수 있다. 단, “F/E 테스트 = only React 컴포넌트 테스트”가 되는 상황은 최대한 피하는 게 좋다. 본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있다. 유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면 오히려 유지보수를 저해할 수 있다._



### Describe-Context-It 패턴

* Describe\
  : 테스트 대상을 설명하는 문구를 작성 👉🏻 "User 모델", "Login 기능"
* Context\
  : Describe에 대한 추가적인 설명을 작성 👉🏻 "로그인 성공 시", "로그인 실패 시"
* It\
  : Context에 대한 구체적인 테스트 케이스를 작성 👉🏻 "로그인 성공 시 세션 정보를 저장해야 한다", "로그인 실패 시 에러 메시지를 출력해야 한다"

{% hint style="info" %}
Describe 단계에서 대상을 정의하고, Context 단계에서 대상에 대한 추가 정보를 제공하며, It 단계에서 실제 테스트를 수행하도록 구성
{% endhint %}

\


### React Testing Library

* 리액트 컴포넌트를 테스팅하기 위한 라이브러리
* 사용자의 시점에서 컴포넌트를 테스트하고, 컴포넌트의 동작을 검증하는 데에 초점을 두고 있음



### BDD

* Behavior-Driven Development
* 애플리케이션의 행위(behavior)를 중심으로 개발하는 방법론
* BDD는 사용자 스토리(user story)나 시나리오(scenario)를 작성하고, 이를 기반으로 테스트 코드를 작성하며, 이를 통해 애플리케이션의 동작을 검증한다.

💬 TDD는 무엇일까?

* Test-Driven Development
* 테스트를 먼저 작성하고, 이를 통과하기 위한 코드를 작성하는 개발 방법론
* TDD는 기능 단위의 테스트에 중점을 두며, 각각의 기능마다 테스트 코드를 작성하고, 이를 통과하기 위한 최소한의 코드를 작성

💬 BDD와 TDD의 차이점은 무엇일까?

> 둘의 가장 큰 차이점은 개발의 관점이다.

* TDD는 개발자가 작성한 코드가 정해진 테스트 케이스를 통과하는지 검증하는 방법
  * 즉, TDD는 개발자 중심의 접근 방식
  * **코드의 동작**을 검증하기 위해 테스트 케이스를 작성
* BDD는 비즈니스 관점에서 소프트웨어의 동작을 검증하는 방법
  * BDD는 테스트 케이스를 작성하는 것뿐만 아니라, 이를 바탕으로 문서를 작성하고, 비즈니스 요구사항을 충족하는 소프트웨어를 개발하는 것을 목표로 한다.
* 테스트 케이스 작성방법 또한 다름
  * TDD: Red-Green-Refactor 패턴
  * BDD: Given-When-Then 패턴

### E2E

* End-to-End
* 소프트웨어 제품의 전체 시스템이나 서비스를 테스트하는 방법을 말한다.
* 실제 사용자가 사용하는 환경과 유사한 환경에서 실행되는 테스트를 의미함
