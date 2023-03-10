## ๐ ๊ฐ์ ์ ๋ฆฌ

## Jest

๐ย [Jest ๊ณต์๋ฌธ์](https://jestjs.io/)

ํ์คํ ๋๊ตฌ

Mocah์ Chai์ฒ๋ผ RSpc์ decribe-it์ ์ง์ํ๊ณ  expect๋ก ๋จ์ธ(assertion)ํ  ์ ์๋ค.

ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํด๋ณธ ๊ฒฝํ์ด ์์ผ๋ ๊ธฐ๋ณธ์ ์ธ ํ์คํธ ์ฝ๋๊ฐ ์ด๋ป๊ฒ ์๊ฒผ๋์ง ๋ณด๊ณ  ๋ง๋ค์ด๋ณด์.

```jsx
// ํ์คํธ ์ฝ๋ ํ์ผ ์์ฑ
touch src/main.test.ts

// ํ์คํธ ์คํ
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

BDD ์คํ์ผ์ ์ฝ๋

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

BDD ์คํ์ผ์ ์ฝ๋๋ฅผ ์์ฑํจ์ผ๋ก์จ ํํ๋ ฅ์ด ์ข์์ง๊ณ  ์๊ฐํ  ์ ์๋ ๊ธฐํ๊ฐ ์ฃผ์ด์ง๋ค.

<br>

## React Testing Library

๐ย [React Testing Library ๊ณต์๋ฌธ์](https://testing-library.com/docs/react-testing-library/intro/)

๐ย [jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)

UI ํ์คํธ์ ํนํ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ

๊ฐ๋จํ ํ์คํธ ์ฝ๋

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

`screen.getByText` ๋ฅผ ์ฌ์ฉํด์ ํ์คํธ๋ฅผ ๊ฐ์ง ์๋ฆฌ๋จผํธ๋ฅผ ์ฐพ์ ์ ์๋ค.

```jsx
test('Greeting', () => {
  render(<Greeting name="world" />);

  screen.getByText('Hello, world!');
});
```

- Greeting ํจ์๋ name=โworldโ๋ฅผ ์ ๋ฌํ์ผ๋ `โHello, world!โ` ๋ฅผ ๋ฆฌํดํ  ๊ฒ์ด๊ณ  ํ์คํธ๋ ์ ์์ ์ผ๋ก ํต๊ณผ๋จ
- ๋ง์ฝ `screen.getByText('Hello, world')` ๋ผ๊ณ  ์์ฑํ๋ฉด? ๐๐ปย ์ ํํ๊ฒ `'Hello, world'` ๋ฅผ ๊ฐ์ง ์๋ฆฌ๋จผํธ๋ฅผ ์ฐพ์ผ๋ ค๊ณ  ํจ
- ์ผ๋ถ๋ง ๊ฐ์ง๊ณ  ์ฐพ๊ณ  ์ถ๋ค๋ฉด ์ด๋ป๊ฒ ํด์ผํ ๊น? ๐๐ปย ์ ๊ทํํ์ ์ฌ์ฉ

  ```jsx
  test('Greeting', () => {
    render(<Greeting name="world" />);

    screen.getByText(/Hello/);
  });
  ```

- `getByText` ๋ ์์ผ๋ฉด ์๋ฌ๋ฅผ ๋ด์ง๋ง, queryByText๋ ์์ ๋ ์๋ฌ๋ฅผ ๋ด์ง ์์

  ```jsx
  expect(scree.queryByText(/Hi/)).toBeFalsy();

  // ์ ์ฝ๋ ๋ณด๋ค ์ข์ ๋ฐฉ๋ฒ
  expect(scree.queryByText(/Hi/)).not.toBeInTheDocument();
  ```

> _React Testing Library๋ ๊ฑฐ์ E2E Test์ฒ๋ผ ์ธ ์ ์๋ค. ๋จ, โF/E ํ์คํธ = only React ์ปดํฌ๋ํธ ํ์คํธโ๊ฐ ๋๋ ์ํฉ์ ์ต๋ํ ํผํ๋ ๊ฒ ์ข๋ค. ๋ณธ์ง์ ์ง์คํ์ง ๋ชปํ๊ณ  ๋๋ฌด ๋ง์ ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ  ์ํ์ด ์๋ค. ์ ์ง๋ณด์๋ฅผ ๋๊ธฐ ์ํด ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๋๋ฐ, ํ์คํธ ์ฝ๋๋ฅผ ์๋ชป ์์ฑํ๋ฉด ์คํ๋ ค ์ ์ง๋ณด์๋ฅผ ์ ํดํ  ์ ์๋ค._

---

## โ Keyword

### Jest

- ํ์ด์ค๋ถ์์ ๊ฐ๋ฐํ ํ์คํธ ํ๋ ์์ํฌ
- Jest ์ด์ ์ ๊ฐ์ฅ ์ธ๊ธฐ๊ฐ ๋ง์๋ ํ์คํธ ๋๊ตฌ๋ Mocha, Jasmine

๐๐ปโโ๏ธ Jest๊ฐ ํ์คํธ ํ๋ ์์ํฌ ์ค ๊ฐ์ฅ ์ธ๊ธฐ๊ฐ ๋ง์ ์ด์ ๊ฐ ๋ญ๊น?

- ์ฌ์ฉ์ด ์ฝ๊ณ  ๊ฐํธํ๋ค.
- Jest๋ ์๋ํ๋ ๋ชจํน์ ์ ๊ณตํ์ฌ, ๋ณ๋์ ๋ชจํน ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ์ง ์์๋ ๋๋ค.
- Jest๋ ๋ณ๋ ฌ ์ฒ๋ฆฌ๋ฅผ ์ง์ํ๋ฏ๋ก, ๋น ๋ฅธ ์๋๋ก ํ์คํธ๋ฅผ ์คํํ  ์ ์๋ค.

<br>

### Describe-Context-It ํจํด

- Describe  
  : ํ์คํธ ๋์์ ์ค๋ชํ๋ ๋ฌธ๊ตฌ๋ฅผ ์์ฑ ๐๐ป "User ๋ชจ๋ธ", "Login ๊ธฐ๋ฅ"

- Context  
  : Describe์ ๋ํ ์ถ๊ฐ์ ์ธ ์ค๋ช์ ์์ฑ ๐๐ป "๋ก๊ทธ์ธ ์ฑ๊ณต ์", "๋ก๊ทธ์ธ ์คํจ ์"

- It  
  : Context์ ๋ํ ๊ตฌ์ฒด์ ์ธ ํ์คํธ ์ผ์ด์ค๋ฅผ ์์ฑ ๐๐ป "๋ก๊ทธ์ธ ์ฑ๊ณต ์ ์ธ์ ์ ๋ณด๋ฅผ ์ ์ฅํด์ผ ํ๋ค", "๋ก๊ทธ์ธ ์คํจ ์ ์๋ฌ ๋ฉ์์ง๋ฅผ ์ถ๋ ฅํด์ผ ํ๋ค"

{% hint style="info" %}
Describe ๋จ๊ณ์์ ๋์์ ์ ์ํ๊ณ , Context ๋จ๊ณ์์ ๋์์ ๋ํ ์ถ๊ฐ ์ ๋ณด๋ฅผ ์ ๊ณตํ๋ฉฐ, It ๋จ๊ณ์์ ์ค์  ํ์คํธ๋ฅผ ์ํํ๋๋ก ๊ตฌ์ฑ
{% endhint %}

<br>

### React Testing Library

- ๋ฆฌ์กํธ ์ปดํฌ๋ํธ๋ฅผ ํ์คํํ๊ธฐ ์ํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ
- ์ฌ์ฉ์์ ์์ ์์ ์ปดํฌ๋ํธ๋ฅผ ํ์คํธํ๊ณ , ์ปดํฌ๋ํธ์ ๋์์ ๊ฒ์ฆํ๋ ๋ฐ์ ์ด์ ์ ๋๊ณ  ์์

---

## ๐ Supplement

### BDD

- Behavior-Driven Development
- ์ ํ๋ฆฌ์ผ์ด์์ ํ์(behavior)๋ฅผ ์ค์ฌ์ผ๋ก ๊ฐ๋ฐํ๋ ๋ฐฉ๋ฒ๋ก 
- BDD๋ ์ฌ์ฉ์ ์คํ ๋ฆฌ(user story)๋ ์๋๋ฆฌ์ค(scenario)๋ฅผ ์์ฑํ๊ณ , ์ด๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๋ฉฐ, ์ด๋ฅผ ํตํด ์ ํ๋ฆฌ์ผ์ด์์ ๋์์ ๊ฒ์ฆํ๋ค.

๐ฌ TDD๋ ๋ฌด์์ผ๊น?

- Test-Driven Development
- ํ์คํธ๋ฅผ ๋จผ์  ์์ฑํ๊ณ , ์ด๋ฅผ ํต๊ณผํ๊ธฐ ์ํ ์ฝ๋๋ฅผ ์์ฑํ๋ ๊ฐ๋ฐ ๋ฐฉ๋ฒ๋ก 
- TDD๋ ๊ธฐ๋ฅ ๋จ์์ ํ์คํธ์ ์ค์ ์ ๋๋ฉฐ, ๊ฐ๊ฐ์ ๊ธฐ๋ฅ๋ง๋ค ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๊ณ , ์ด๋ฅผ ํต๊ณผํ๊ธฐ ์ํ ์ต์ํ์ ์ฝ๋๋ฅผ ์์ฑ

๐ฌ BDD์ TDD์ ์ฐจ์ด์ ์ ๋ฌด์์ผ๊น?

> ๋์ ๊ฐ์ฅ ํฐ ์ฐจ์ด์ ์ ๊ฐ๋ฐ์ ๊ด์ ์ด๋ค.

- TDD๋ ๊ฐ๋ฐ์๊ฐ ์์ฑํ ์ฝ๋๊ฐ ์ ํด์ง ํ์คํธ ์ผ์ด์ค๋ฅผ ํต๊ณผํ๋์ง ๊ฒ์ฆํ๋ ๋ฐฉ๋ฒ

  - ์ฆ, TDD๋ ๊ฐ๋ฐ์ ์ค์ฌ์ ์ ๊ทผ ๋ฐฉ์
  - **์ฝ๋์ ๋์**์ ๊ฒ์ฆํ๊ธฐ ์ํด ํ์คํธ ์ผ์ด์ค๋ฅผ ์์ฑ

- BDD๋ ๋น์ฆ๋์ค ๊ด์ ์์ ์ํํธ์จ์ด์ ๋์์ ๊ฒ์ฆํ๋ ๋ฐฉ๋ฒ

  - BDD๋ ํ์คํธ ์ผ์ด์ค๋ฅผ ์์ฑํ๋ ๊ฒ๋ฟ๋ง ์๋๋ผ, ์ด๋ฅผ ๋ฐํ์ผ๋ก ๋ฌธ์๋ฅผ ์์ฑํ๊ณ , ๋น์ฆ๋์ค ์๊ตฌ์ฌํญ์ ์ถฉ์กฑํ๋ ์ํํธ์จ์ด๋ฅผ ๊ฐ๋ฐํ๋ ๊ฒ์ ๋ชฉํ๋ก ํ๋ค.

- ํ์คํธ ์ผ์ด์ค ์์ฑ๋ฐฉ๋ฒ ๋ํ ๋ค๋ฆ
  - TDD: Red-Green-Refactor ํจํด
  - BDD: Given-When-Then ํจํด

### E2E

- End-to-End
- ์ํํธ์จ์ด ์ ํ์ ์ ์ฒด ์์คํ์ด๋ ์๋น์ค๋ฅผ ํ์คํธํ๋ ๋ฐฉ๋ฒ์ ๋งํ๋ค.
- ์ค์  ์ฌ์ฉ์๊ฐ ์ฌ์ฉํ๋ ํ๊ฒฝ๊ณผ ์ ์ฌํ ํ๊ฒฝ์์ ์คํ๋๋ ํ์คํธ๋ฅผ ์๋ฏธํจ
