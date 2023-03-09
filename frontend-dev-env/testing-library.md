## 📚 강의 정리

## Jest

🚀 [Jest 공식문서](https://jestjs.io/)

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
		expect(add(1, 2).toBe(3);
	});

	it('returns number', () => {
		expect(typeof add(1, 2)).toBe('number');
	});
});
```

BDD 스타일의 코드를 작성함으로써 표현력이 좋아지고 생각할 수 있는 기회가 주어진다.

<br>

## React Testing Library

🚀 [React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro/)

🚀 [jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)

UI 테스트에 특화된 라이브러리

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

- Greeting 함수는 name=”world”를 전달했으니 `‘Hello, world!’` 를 리턴할 것이고 테스트는 정상적으로 통과됨
- 만약 `screen.getByText('Hello, world')` 라고 작성하면? 👉🏻 정확하게 `'Hello, world'` 를 가진 엘리먼트를 찾으려고 함
- 일부만 가지고 찾고 싶다면 어떻게 해야할까? 👉🏻 정규표현식 사용

  ```jsx
  test('Greeting', () => {
    render(<Greeting name="world" />);

    screen.getByText(/Hello/);
  });
  ```

- `getByText` 는 없으면 에러를 내지만, queryByText는 없을 때 에러를 내지 않음

  ```jsx
  expect(scree.queryByText(/Hi/)).toBeFalsy();

  // 위 코드 보다 좋은 방법
  expect(scree.queryByText(/Hi/)).not.toBeInTheDocument();
  ```

> _React Testing Library는 거의 E2E Test처럼 쓸 수 있다. 단, “F/E 테스트 = only React 컴포넌트 테스트”가 되는 상황은 최대한 피하는 게 좋다. 본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있다. 유지보수를 돕기 위해 테스트 코드를 작성하는데, 테스트 코드를 잘못 작성하면 오히려 유지보수를 저해할 수 있다._

---

## ✅ Keyword

### Jest

### Describe-Context-It 패턴

### React Testing Library

---

## 🐋 Supplement

### RSpec

### BDD

### E2E
