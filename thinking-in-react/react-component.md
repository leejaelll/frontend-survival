# React Component

## 📚 강의 정리

## Thinking in React

> [Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- “Step 1: Break the UI into a component hierarchy”  
  : UI를 컴포넌트 계층구조로 쪼개기 _(DOM트리를 만들듯이 컴포넌트 트리를 만드는 것)_
- “Step 2: Build a static version in React”  
  : React로 정적인 버전 만들기 _(동적인 부분들은 나중에 만들기. Step 2에서는 정적인 것들부터 만든다.)_

---

## 데이터

- 리액트로 컴포넌트를 만들기 전에 가지고 있어야 하는 것 → JSON API & mockup
- 백엔드에서 JSON 형태로 데이터를 돌려주는 API를 제공한다고 가정
- 대부분은 REST API 또는 GraphQL

---

**REST API**

- GET, POST, PUT/PATCH, DELETE (CRUD)
- Resource 중심

**GraphQL**

- Graph 자료 구조
- Query에서 얻고자 하는 걸 지정
- Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

F/E는 이 데이터를 사용자가 볼 수 있도록 UI를 구성한다. React는 선언형(HTML과 유사한 모양의 DSL을 사용)으로 UI를 구성할 수 있다.

- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSON 개요](https://www.json.org/json-ko.html)
- [JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
- [명령형 프로그래밍](https://ko.wikipedia.org/wiki/명령형_프로그래밍)
- [선언형 프로그래밍](https://ko.wikipedia.org/wiki/선언형_프로그래밍)

---

## 컴포넌트 계층 구조

> [React](https://reactjs.org/)

React의 강력한 특징 둘 중 하나:

- “Component-Based”
- “Build encapsulated components that manage their own state, then **compose** them to make **complex UIs**.”
- 간단한 컴포넌트의 조립으로 복잡한 UI를 구성한다.
- 반대로 말하면 컴포넌트 하나하나는 복잡하지 않아야 한다.

`간단한 컴포넌트`의 몇 가지 기준:

- [SRP (Single Responsibility Principle)](https://ko.wikipedia.org/wiki/단일_책임_원칙)
- 컴포넌트가 너무 커지고 있다면…
- CSS → 이미 알고 있는 기준을 재활용.
- Design’s Layer
- Information Architecture (JSON Schema의 영향) → 실제로 엄청 많이 쓰게 됨. 자연스러운 SRP를 위해서 사실상 강제됨.

---

## Extract Function

> [Extract Function](https://refactoring.com/catalog/extractFunction.html)

> [Inline Function](https://refactoring.com/catalog/inlineFunction.html)

- 아주 흔히 쓰이는 SRP를 위한 수단. 변화의 크기(영향 범위)를 제약한다.
- 일단 길게 코드를 작성하고, 적절히 자를 수 있는 부분이 보일 때 “함수로 추출”한다.
- 또는 코드를 작성하기 어려운 상황에 직면했을 때 함수로 추출. 바로 다른 파일을 만들어야 한다고 생각하지 않아도 됨.
- 컴포넌트 나누는 기준이 애매하면 다시 하나의 컴포넌트로 합쳤다가(Inline Method) 다시 나눠줘도 됨.

---

## Props

> [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)

> [Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)

- 나눠진 컴포넌트를 서로 연결하는 방법.
- TypeScript를 잘 쓰거나 잘못 쓰게 되는 포인트 중 하나. 적절한 균형점을 잡는 게 중요하다.
- 테스트 코드를 작성하면 재사용성을 평가하기 쉬워짐.

강의 코드로 예를 들면,

```jsx
export default function App() {
  return (
    <div className="filterable-product-table">
      <div className="seach-bar">
        <div>
          <input type="text" placeholder="Search..." />
        </div>
        <div>
          <input type="checkbox" id="only-stock" />
          <label htmlFor="only-stock">Only Show products in stock</label>
        </div>
      </div>

      <ProductTable products={products} />
    </div>
  );
}
```

- App 컴포넌트에서 ProductTabel에 products라는 prop을 전달해주었다.
- 그리고 PrdouctTabel에서 products를 받아서 사용한다.

```jsx
function ProductTable({ products }: { products: Product[] }) {
  // 여기서 products에 뭐가 들어오는지 콘솔을 찍어보면? -> 넘겨준 products 배열을 받을 수 있다.
  console.log(products);
  //  [
  //   { category: 'Fruits', price: '$1', stocked: true, name: 'Apple' },
  //   { category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit' },
  //   { category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit' },
  //   { category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach' },
  //   { category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin' },
  //   { category: 'Vegetables', price: '$1', stocked: true, name: 'Peas' },
  // ];
  // ...(생략)...
}
```

---

## ✅ Keyword

## REST API 와 GraphQL

1. REST API 란 무엇인가?

- Representational State Transfer API
- 웹 서비스를 위한 아키텍쳐 스타일 중 하나로, 클라이언트와 서버 간의 통신을 위한 인터페이스
- REST API는 REST 아키텍처 스타일의 디자인 원칙을 준수하는 API. 이러한 이유 때문에 REST API를 종종 RESTfull API라고도 한다.
- 컴퓨터 과학자인 Roy Fielding 박사가 2000년에 자신의 박사 학위 논문에서 처음으로 정의함.

REST 디자인 원칙

- 균일한 인터페이스: 요청이 어디에서 오는지와 무관하게, 동일한 리소스에 대한 모든 API 요청은 동일하게 보여야 한다.
- 클라이언트-서버 디커플링: 클라이언트와 서버 애플리케이션은 서로 간에 완전히 독립적이어야 한다.
- Stateless: 서버 애플리케이션은 클라이언트 요청과 관련된 데이터를 저장할 수 없다.
- 계층 구조 아키텍처: REST API는 엔드 애플리케이션 또는 중개자와 통신하는지 여부를 클라이언트나 서버가 알 수 없도록 설계되어야 한다.

- 출처 [IBM](https://www.ibm.com/kr-ko/cloud/learn/rest-apis)

---

2. GraphQL은 왜 등장했는가?

GrapQL이란?

- Facebook에서 개발한 쿼리 언어 👉🏻 쿼리 언어라는게 뭐지?

> 쿼리 언어란, 데이터베이스나 API와 같은 데이터 소스에서 원하는 데이터를 추출하기 위해 사용되는 언어

- RESTful API와 같은 API 디자인 패턴 대신에, 클라이언트가 데이터 요청을 할 때 원하는 데이터의 구체적인 필드를 지정하여 원하는 데이터만을 가져올 수 있도록 한다. _(즉, 웹 클라이언트가 데이터를 서버로부터 효율적으로 가져오는 것이 목적!)_
- 출처 [kakao Tech](https://tech.kakao.com/2019/08/01/graphql-basic/)

GrapQL은 왜 등장했을까?

- Facebook은 기존에 RESTful API를 사용했었는데, RESTful API로는 클라이언트가 필요한 데이터를 효율적으로 가져오기는 어렵다는 문제를 발견함. _(REST API는 클라이언트가 데이터를 요청할 때마다 서버는 모든 필드를 반환하기 때문에 불필요한 데이터가 많아짐.)_
- 이러한 문제를 해결하기 위해 GraphQL을 개발
- GraphQL은 클라이언트가 필요한 데이터를 직접 선택하여 가져올 수 있도록 하여, 불필요한 데이터를 줄이고 성능을 향상시킴

---

3. REST API vs GraphQL
   > REST API와 GraphQL의 기본적인 차이는 데이터 요청방법!

- REST API에서는 클라이언트 서버에 HTTP 요청을 보내면 서버는 그에 따른 HTTP 응답을 반환한다.
- 이 때 클라이언트가 요청하는 데이터의 구체적인 필드를 지정할 수 없으며, 서버는 모든 필드를 반환한다.
- 반면 GraphQL에서는 클라이언트가 서버에 GraphQL의 쿼리를 보내면 그에 맞게 필드를 선택하여 반환한다.

사용자 정보를 가져오는 코드로 비교해보면,
REST API에서는 다음과 같이 작성한다.

```jsx
GET / users / 1;
```

- 이 요청을 보내면 서버는 ID가 1인 사용자의 모든 정보를 반환한다.

GraphQL에서는 사용자 정보를 가져올 떄 다음과 같이 쿼리를 보낼 수 있다.

```jsx
query {
  user(id: 1) {
      name
      email
      profilePicture
  }
}
```

- 이 요청을 보내면 서버는 ID가 1인 사용자의 이름, 이메일 주소 및 프로필 사진만을 반환한다.

또 다른 차이점으로는 URL구조, 데이터 일관성 유지, 응답 시간이 있다.

- URL 구조

  - REST API: 각 엔드포인트는 URL 구조를 따르며, HTTP 메서드(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD 작업을 수행한다.
  - GraphQL: 단일 엔드포인트를 사용하며, 쿼리와 뮤테이션을 사용하여 데이터를 검색 및 조작한다.

  ```jsx
  // RESTful API
  app.get('/users/:user_id', (req, res) => {
    const user_id = req.params.user_id;
    User.findById(user_id, (err, user) => {
      if (err) {
        return res.status(404).json({ error: 'User not found' });
      }
      res.json(user);
    });
  });

  // GraphQL
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      username
      email
      posts {
        id
        title
      }
  }
  }
  ```

- 데이터 일관성 유지

  - REST API: 데이터 일관성은 서버 측에서 관리. 클라이언트가 데이터를 조작할 때는 서버의 규칙을 따라야 한다.
  - GraphQL: 타입 시스템을 사용하여 일관성을 유지한다. 클라이언트가 쿼리를 작성할 때 적절한 데이터 타입을 사용하도록 강제된다.

---

## JSON

- JSON (JavaScript Object Notation)은 경량 데이터 교환 형식
- 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- 더글라스 크록포드가 규정
- JSON은 텍스트 형식으로 이루어져 있으며, 데이터 객체와 배열을 표현하는 데 사용
- JSON 형식은 다양한 프로그래밍 언어에서 사용할 수 있으며, 특히 웹 어플리케이션에서 API 데이터를 전송하는 데 많이 사용된다.
- JSON은 자바스크립트에서 객체를 표현하는 방식을 차용 _(SON 객체는 중괄호({})로 둘러싸여 있으며, 속성-값 쌍)_
- 속성 이름은 문자열이며, 값은 문자열, 숫자, 불리언, null, 배열, 객체 등의 데이터 타입될 수 있다.

```jsx
{
  "name": "John",
  "age": 30,
  "isMarried": true,
  "hobbies": ["reading", "music", "swimming"],
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001"
  }
}
```

JSON을 사용하면 얻을 수 있는 이점은?

- 가독성이 뛰어나다.  
  : JSON 형식은 객체리터럴 형식을 사용하기 떄문에 사람이 쉽게 읽고 쓸 수 있다.
- 데이터 전송 및 교환이 쉽다.  
  : JSON 형식은 인터넷에서 데이터를 교환하는데 매우 효과적이다.HTTP 통신을 비롯한 여러 통신 프로토콜에서 JSON을 사용할 수 있으며, 데이터를 효율적으로 압축할 수 있다.
- 다양한 프로그래밍 언어에서 지원한다.  
  : 자바스크립트에서 시작되었지만, 다양한 프로그래밍 언어에서 지원된다.

---

## DSL(Domain-Specific Language)

- 특정 도메인이나 문제 영역에 특화된 언어
- 이러한 언어는 특정 도메인의 문제를 해결하기 위해 설계되었으며, 해당 도메인의 전문가들이 이해하기 쉽고 효과적으로 사용할 수 있다.

리액트에서 DSL은?

- 리액트에서 DSL은 JSX
- 즉, JSX를 사용해 XML 형태로 UL를 표현할 수 있게 해주는 DSL이다.

---

## 선언형 프로그래밍

- 프로그램이 어떻게 동작해야 하는지를 기술하는 것이 아니라, 원하는 결과를 기술하는 프로그래밍 패러다임 중 하나
- 프로그램이 *어떤 방법(How)*으로 해야 하는지를 나타내기보다 *무엇(What)*과 같은지를 설명하는 경우에 "선언형(declarative)"이라고 한다.
- 예를 들어, 웹 페이지는 선언형인데 웹페이지는 제목, 글꼴, 본문, 그림과 같이 "무엇"이 나타나야하는지를 묘사하는 것이지 "어떤 방법으로" 컴퓨터 화면에 페이지를 나타내야 하는지를 묘사하는 것이 아니기 때문
- 선언형 프로그래밍에서는 일련의 변환을 통해 원하는 결과를 얻어냄
  - 이를 위해 자바스크립트에서는 배열과 같은 컬렉션 타입에 대한 메서드를 활용 (map, reduce, filter)

선언형 프로그래밍의 장단점

- 코드의 가독성이 높아지며, 유지보수가 쉬워진다.
- 부작용(side effect)을 최소화하여 예측 가능한 동작을 보장할 수 있다.
- 선언형 프로그래밍에서는 추상화 수준이 높아지므로 초기 학습비용이 높을 수 있다.

## 명령형 프로그래밍

- 프로그램이 어떻게 동작해야 하는지를 명시적으로 지정한다. _(즉, 명령형 프로그래밍에서 컴퓨터가 수행해야 할 명령문들의 목록을 명시한다.)_
- 프로그램이 수행해야 할 작업을 순차적으로 기술
- 프로그래밍 상태를 직접 변경하는 방식으로 동작한다.
- 자바스크립트에서는 조건문(if-else, switch), 반복문(for, while) 등의 구문을 사용한다.

명령형 프로그램의 장단점

- 코드를 작성하기에 비교적으로 쉽다.
- 하지만 코드가 길어지고 복잡해지는 경우, 유지보수가 어렵다.
- 프로그램의 상태를 직접 변경하는 방식은 부작용을 일으킬 수 있다는 단점이 있다.

## SRP(단일 책임 원칙)

- 객체 지향 프로그래밍에서 중요한 원칙 중 하나
- 로버트 C. 마틴이 객체 지향 설계의 원칙의 일부인 “OOD의 원칙”이라는 글에서 소개
- ‘클래스나 모듈은 하나의 액터에게만 책임을 져야한다.’는 컴퓨터 프로그래밍 원칙
  - 액터라는 용어는 모듈을 변경해야하는 그룹을 의미 (한 명 이상의 이해관계자 또는 사용자로 구성됨)
  - The term actor refers to a group (consisting of one or more stakeholders or users) that requires a change in the module.
- 로버트 C. 마틴은 이 원칙을 ‘클래스를 변경할 이유가 하나만 있어야 한다.’라고 표현
- “원인(이유)”이라는 단어 때문에 혼란이 생겨 “원칙은 사람에 관한 것이다”라고 명확히 함.
- 예를 들어, 주문 클래스는 👉🏻 주문을 처리하는 책임만 가지고 있어야 한다.
  - 결제나 배송과 같은 책임은 다른 클래스에서 처리되어야 함
  - 이렇게 되면 주문 클래스는 주문과 관련된 기능만을 가지고 있고 코드의 복잡도가 감소하여 유지보수가 쉬워진다.

SRP의 장점

- 단일 책임 원칙을 지키면 클래스 간 의존성이 감소하고, 결합도가 낮아져 시스템의 유연성과 확장성이 향상된다.

---

## Atomic Design

- Atomic Design은 웹 디자인과 프론트엔드 개발을 위한 디자인 시스템으로서, 컴포넌트 기반 디자인 시스템의 한 형태
- Atomic Design은 웹 디자인을 구성하는 모든 요소를 다섯 가지의 계층으로 분류 👉🏻 이를 기반으로 컴포넌트 단위로 디자인과 개발을 진행

  1. 아톰(Atom): 웹 디자인을 구성하는 가장 작은 요소 _(Input, label등과 같은 단순한 요소)_
  2. 분자(Molecule): 두 개 이상의 아톰이 결합하여 형성되는 복합적인 요소 _(Search Form, Login Form)_
  3. 조직(Organism): 다양한 아톰과 분자, 그리고 다른 조직을 조합하여 형성되는 요소 _(header, footer, sidebar)_
  4. 템플릿(Template): 구체적인 레이아웃과 구성 요소를 결합하여 구성한 페이지의 기본 뼈대를 형성하는 요소
  5. 페이지(Page): 실제 사용자가 볼 수 있는 완성된 웹 페이지를 나타내는 요소

- Atomic Design을 통해 재사용성과 확장성을 높일 수 있고, 일관성있는 디자인 시스템을 구축할 수 있다.

---

## React component 와 props

- component와 component 간에 공통적으로 사용해야 할 정보들이 있을 수 있음
- 이 때 부모 컴포넌트는 props로 자식 컴포넌트에게 자식 컴포넌트가 사용할 수 있도록 정보를 제공할 수 있다.
- props는 컴포넌트에 전달되는 데이터이므로 컴포넌트 내부에서 변경이 불가능하다.
  - 컴포넌트 내부에서 상태를 관리하려면 state 내부 상태를 사용한다.
- 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 함. 즉 컴포넌트 자체 props를 수정해서는 안 됨(props는 읽기 전용)

---

## Supplement

## 타입은 언제 쓰고 interface는 언제 쓰는걸까?
