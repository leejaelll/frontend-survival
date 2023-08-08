# React Testing Library

## 학습 키워드

* React Testing Library
* given - when - then 패턴
* Mocking
* Test fixture



* [React Testing Library](https://github.com/testing-library/react-testing-library)
* [jest-dom](https://github.com/testing-library/jest-dom)

{% hint style="info" %}
### React Testing Library

좋은 테스트 관행을 장려하는 간단하고 완벽한 React DOM 테스트 유틸리티

#### ✅ React Testing Library 역할

테스트를 위한 가상 DOM을 생성하고, DOM과 상호 작용하기 위한 유틸리티도 제공한다. 예를 들어, DOM에서 요소를 찾을 수 있거나, 클릭과 같은 요소와 상호작용할 수 있으며, 브라우저 없이도 테스트를 할 수 있도록 한다.

Jest와 같은 테스트 프레임워크와 함께 사용될 수 있도록 설계되어있음

* 브라우저 내 테스트, 서버사이드 렌더링 테스트 등 다양한 환경에서 테스트를 하는데 사용할 수 있다.
* 개발자가 구현 세부 사항이 아닌 “애플리케이션의 동작”에 초점을 맞춘 테스트를 작성하도록 권장
{% endhint %}

```jsx
test('renders learn react link', () => {
  render(<App />); // 🚧 가상DOM 생성, 이 가상 DOM에는 어떻게 접근할까? 👉🏻 screen이라는 글로벌 객체로 접근
  const linkElement = screen.getByText(/learn react/i); // 🚧 getByText 메서드의 인수로는 정규표현식이 들어가야 함.
  expect(linkElement).toBeInTheDocument(); // 🚧 Assertion, 테스트 성공과 실패의 원인
});
```

만약 getByText 메서드에 전달한 문자열이 컴포넌트에 없다면 오류를 보여준다.

> TestingLibraryElementError: Unable to find an element with the text: /learn testing library/i. This could be because the text is broken up by multiple elements. In this case, you can provide a function for your text matcher to make your matcher more flexible.

\


{% hint style="success" %}
### jest-dom

Jest 테스트 프레임워크용 확장 라이브러리

* DOM 요소 및 상호작용 테스트를 간소화하기 위해 기능을 제공한다.
* 요소가 표시되는지, 비활성화되었는지 또는 특정 텍스트가 포함되어 있는지 확인하는 것과 같은 일반적인 작업을 위한 matchers.
* 버튼을 클릭하거나 입력 필드에 텍스트를 입력하는 것과 같은 DOM 요소와의 사용자 상호 작용을 시뮬레이션하기 위한 유틸리티 기능.
* 실패한 테스트의 원인을 더 쉽게 식별할 수 있도록 개선된 오류 메시지
{% endhint %}

### 🦖 jest-dom과 jest를 함께 사용했을 때의 장점

웹 애플리케이션에 대한 포괄적인 테스트를 생성할 수 있으므로, 개발 프로세스 초기에 버그와 문제를 파악하는 데 도움이 될 수 있다. 또한 jest-dom은 사용자 정의가 가능해서 특정 프로젝트의 요구사항에 맞게 기능을 쉽게 확장할 수 있다.

\


test 파일은 `filename.test.tsx` 로 컴포넌트와 동일한 위치에 두기

```tsx
// ✅ @testing-library/react의 render를 import
import { render } from '@testing-library/react';

test('TextField', () => {
  // given
  const setFilterText = () => {}

  // when
  render((
    <TextField
      placeholder=""
      filterText=""
      setFilterText={setFilterText}  // ✅ 해당 함수는 위에서 정의
    />

  // then
  screen.getByLabelText('Search'); // label text가 'Search'인 것을 찾는다.
  ))
});
```

\


### 🦖 `screen.getByLabelText`의 사용 방법

: input field와 같은 form control 동작을 테스트해야할 때 유용하다.

```tsx
<label htmlFor="username-input">Username:</label>
<input id="username-input" type="text" />
```

연결된 레이블을 사용하여 input field를 선택하려면 \*\*`screen.getByLabelText`\*\*를 사용한다.

```tsx
const usernameInput = screen.getByLabelText('Username:');
```

`getByLabelText` 함수는 전체 문서에서 form 컨트롤의 `id`와 일치하는 `for` 속성이 있는 `label` 요소가 있는 form 컨트롤 또는 일치하는 텍스트가 있는 중첩된 `label` 요소를 검색한다.

_(이 경우 `id="username-input"`인 `input` 요소를 선택)_

레이블 텍스트가 변경되지 않는 한 getByLabelText를 사용하여 form 컨트롤을 사용하면, 애플리케이션의 구조 또는 레이아웃 변경에 대한 테스트의 탄력성을 높일 수 있다.

### 🦖 08:55 - TextField가 범용이어야 한다는게 무슨 의미지?

TextField라는 컴포넌트는 여러군데에서 사용할 수 있어야한다. 하지만 현재 TextField 컴포넌트는 label이 “Search”로 고정되어 있다. 범용적으로 사용하려면 label의 값도 prop으로 전달해줘야 한다는 것!

```tsx
<TextField
  label="Search" // ✅ props로 전달하자
  placeholder="Input your name"
  filterText={text}
  setFilterText={setText}
/>
```

\


filterText 또한 이상하게 느껴진다. 그저 text면 되는데?

```jsx
import { render, screen } from '@testing-library/react';

import TextField from './TextField';

test('TextField', () => {
  const text = 'Tester';
  const setText = () => {
    // do nothing...
  };

  render(
    <TextField
      label="Name"
      placeholder="Input your name"
      text={text}
      setText={setText}
    />
  );

  screen.getByLabelText('Name');
});
```

테스트 코드, 즉 컴포넌트를 사용하는 코드를 작성하면서 해당 컴포넌트의 인터페이스를 점검할 수 있다. 기존에는 label이 빠져있었고, text 같이 범용적인 표현을 사용하지 않은 문제가 있었다.

개발하면서 이런 문제를 발견할 수도 있지만, 우리가 테스트부터 작성했거나 빠르게 테스트 코드를 작성했다면 작성하기 전 또는 바로 직후에 문제를 발견해서 수정할 수 있었을 것.

시간이 지나면 해당 코드에 대한 지식이 감소하고, 자신감 또한 감소하기 때문에 건드리기 힘든 코드가 되기 십상이다.

BDD 스타일로 코드를 바꾸고, 입력 등이 잘 작동하는지 확인해 보자.

```jsx
import { render, screen, fireEvent } from '@testing-library/react';

import TextField from './TextField';

**const context = describe;**

describe('TextField', () => {
  const text = 'Tester';
  const setText = **jest.fn()**;

  beforeEach(() => {
    setText.**mockClear**();
    // 또는 **jest.clearAllMocks()**;
  });

  function renderTextField() {
    render((
      <TextField
        label="Name"
        placeholder="Input your name"
        text={text}
        setText={setText}
      />
    ));
  }

  it('renders an input control', () => {
    renderTextField();

    screen.getByLabelText('Name');
  });

  context('when user types text', () => {
    it('calls the change handler', () => {
      renderTextField();

      **fireEvent**.change(screen.getByLabelText('Name'), {
        target: {
          value: 'New Name',
        },
      });

      expect(setText).**toBeCalledWith**('New Name');
    });
  });
});
```

반복되는 코드를 Extract Function하고, fireEvent 등을 통해 인터랙션만 검증한다. 만약 복잡한 로직이 컴포넌트로부터 분리된다면, 여기서는 이것만 검증하면 된다.

{% hint style="info" %}
### 🦁 jest.fn()의 역할

* jest.fn() 함수는 Jest 테스트 프레임워크에서 제공하는 유틸리티
* 실제 함수를 대체하기 위해 테스트에서 사용할 수 있는 일종의 가상의 함수

jest.fn()을 호출하면 새로운 빈 함수를 반환한다. 이 mock 함수는 테스트 실행 중에 동작을 관찰하고 제어할 수 있다.

간단한 예를 들어보면, text state를 변경하는 setText 함수가 호출되는지 테스트해야하는 경우가 있다. 이 때, setText가 무엇을 하는지는 중요하지 않고, 불렸는지만 확인하고 싶을 때 jest.fn()을 사용한다.

```jsx
setText = jest.fn();

expect(setText).toBeCalled();
```

#### 🦁 jest.fn을 사용하는 이유

jest.fn()을 사용하는 것은 테스트 중에 코드에서 특정 기능이나 종속성을 분리하려는 경우에 특히 유용하다. 이를 통해 이러한 함수의 동작을 제어하고 실제 구현을 실행하지 않고도 함수가 사용되는 방식을 확인할 수 있다.

이렇게 하면 예측 가능한 시나리오를 만들고 코드가 mock 함수와 올바르게 상호 작용하는지 확인할 수 있으므로 테스트 안정성이 향상된다.

#### 🚧 테스트가 여러 개인 경우 jest.fn을 초기화해줘야 한다

```jsx
describe('TextField', () => {
  const text = 'Tester';
  const setText = jest.fn();

  function renderTextField() {
    render(
      <TextField
        label="Name"
        placeholder="Input your name"
        text={text}
        setText={setText}
      />
    );
  }

  it('renders an input control', () => {
    renderTextField();
    screen.getByLabelText('Name');
  });

  context('when user types text', () => {
    it('calls the change handler', () => {
      renderTextField();
      fireEvent.change(screen.getByLabelText('Name'), {
        target: {
          value: 'New Name',
        },
      });

      expect(setText).toBeCalledWith('New Name');
    });
  });
});
```

setText를 여러 군데에서 사용하는 경우에는 한번이라도 함수가 호출된 경우 불렸다는 정보를 저장하고 있기 때문에 매번 초기화를 시켜줘야한다.

```jsx
beforeEach(() => {
  setText.mockClear();
});

// 또는

beforeEach(() => {
  jest.clearAllMocks();
});
```
{% endhint %}

\


### 🦖 `fireEvent`의 사용 방법

: 버튼 클릭, input field 또는 submitting a form과 같은 DOM 요소와의 사용자 상호 작용을 시뮬레이션할 수 있는 유틸리티 기능

: 가상 DOM에서 요소와 상호 작용할 수 있도록 돕는 객체

fireEvent를 사용하면 사용자 동작에 대한 애플리케이션의 동작을 직접 수동으로 수행하지 않고도 테스트할 수 있다.

* fireEvent.click
* fireEvent.sumbit
* fireEvent.change

버튼 클릭을 시뮬레이션하는 예제:

```tsx
import { render, fireEvent } from '@testing-library/react';

test('clicking the button triggers the expected behavior', () => {
  const handleClick = jest.fn();
  const { getByText } = render(<button onClick={handleClick}>Click me</button>);
  const button = getByText('Click me');
  fireEvent.click(button);
  expect(handleClick).toHaveBeenCalled();
});
```

만약 외부 의존성이 큰 코드를 작성한다면, 해당 부분만 가짜로 구현할 수 있다.\
_(예를 들어, 받아오는 데이터엔 'Apple'이라는 상품이 없을 수도 있지만, 있는 것처럼 구현해놓는 것)_

```jsx
jest.mock('모듈 경로', () => () => [
  {
    // 데이터
    id: 1,
  },
]);
// 중첩함수를 사용해야한다.
```

```jsx
import { render, screen } from '@testing-library/react';

import App from './App';

jest.mock('./hooks/useFetchProducts', () => () => [
  {
    category: 'Fruits',
    price: '$1',
    stocked: true,
    name: 'Apple',
  },
]);

test('App', () => {
  render(<App />);

  screen.getByText('Apple');
});
```

👉🏻 일반적으론 백엔드와 소통하는 부분이 차지하는 비중이 큰데, 이 부분을 하나씩 가짜 구현으로 바꾸다 보면 어려울 때가 있다. 이럴 땐 MSW 등 다른 대안을 고려해 보자.

\


{% hint style="info" %}
#### Mocking

: ‘가짜로 만들어진 어떤 것’이라는 의미를 가지고 있다.

테스트 대상의 객체에 **의존되어 있는 다른 객체들을 페이크 객체**로 만드는 것

예를 들어, 회원 가입을 하기 위해서는 ID, PW를 저장하면 되지만 실제로는 ID 중복체크, 비밀번호 유효성 검사 등 하나의 행동에 다양한 의존 관계가 존재한다.

하지만 테스트에서는 ID, PW가 저장되는지만 알고싶다면 어떻게 해야할까?

이를 위해서는 ID 중복체크는 무조건 True, 비밀번호 유효성 검사도 무조건 True, 암호는 123으로 만들어 준다고 가정해보자. 그렇다면 DB에 저장되기 전 객체의 데이터가 { ID: ‘minsoo’, PW: 123}라면 정상적으로 회원 가입 로직을 수행한 것으로 볼 수 있다.

_**위에서 회원 가입이라는 행동에 의존된 것들을 가짜 행위한다고 가정한 것이 Mocking이다.**_

이처럼 Mocking은 테스트 대상 객체의 행동을 고립시키기 위해서, 의존성을 배제하기 위해서는 의존되어있는 객체들의 행동을 가짜로 만들어주는 것이다.
{% endhint %}

\


{% hint style="info" %}
### ✅ Test Fixture

: '테스트를 위해 고정되어 있는 것’

무엇을, 왜 고정시켜야하는걸까?

외부 DB나, API 등 대상 서버의 네트워크, 정기작업 이슈가 있다면, 우리는 테스트를 진행할 수 없다. 이를 개선하기 위해 변경되지 않는 상태나 데이터를 미리 만들어 두는 작업을 'Test Fixture'를 만든다고 한다.

_**즉, 테스트를 위해 변경되지 않는 상태의 객체나 데이터를 만들어 둔 것을 'Fixture'라고 한다.**_

#### ✅ Fixture 사용방법

* fixtures 폴더를 만들고 가상의 데이터를 가지고 있는 파일을 만든다.

```jsx
// fixtures/products.ts
const products = [
  { category: 'Fruits', price: '$1', stocked: true, name: 'Apple' },
];

export default products;

// fixtures/index.ts
import products from './products';

export default {
  products,
}
```

```jsx
// App.test.tsx
import fixtures from '../fixtures';

jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);

test('App', () => {
  render(<App />);

  screen.getByText('Apple');
});
```
{% endhint %}
