# TSyringe

## 학습 키워드

- TSyringe
- 의존성 주입(Dependency Injection)
- reflect-metadata
- sington (싱글톤)

---

## TSyringe

- [TSyringe](https://github.com/microsoft/tsyringe)

- [reflect-metadata](https://github.com/rbuckton/reflect-metadata)

- [The problem with passing props](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

{% hint style="info"%}

### TSyringe

: TypeScript용 DI 라이브러리

우리가 TSyringe를 사용하는 주 목적: external store를 관리하는데 사용한다.

'Props Drilling' 문제를 해결할 수 있으며, 전역처럼 사용이 가능하다.

<br />

**DI(Dependency Injection)**
: 종속성 주입

DI를 사용하면 클래스와 개체가 서로 밀접하게 결합되지 않고 런타임 시 개체에 종속성이 "주입"되므로 애플리케이션이 더 모듈화되고 유지 관리가 쉬워진다.

<br />

**DI를 사용해야하는 이유**

- Unit Test가 용이해진다.
- 코드의 재활용성을 높여준다.
- 객체 간의 의존성(종속성)을 줄이거나 없엘 수 있다.
- 객체 간의 결합도이 낮추면서 유연한 코드를 작성할 수 있다.

{% endhint %}

<br />

### reflect-metadata

Reflect API를 위한 폴리필을 추가할 때 사용

reflect-metadata를 사용할 수도 있고 공식 문서에서는 core-js, reflection과 같은 useage도 제공

데코레이터는 선언적 구문을 통해 클래스가 정의될 때 클래스와 그 멤버를 보강하는 기능을 추가

<br />

### singleton(싱글톤)

TSyringe에서의 singleton은 클래스를 전역 컨테이너 내에서 싱글톤으로 등록하는 클래스 데코레이터 팩토리

### 🦖 이전에 디자인 패턴에서 singleton 패턴을 본 적이 있는데 같은 개념인가?

[singleton 패턴](https://patterns-dev-kr.github.io/design-patterns/singleton-pattern/)

Singleton은 1회에 한하여 인스턴스화가 가능하며 전역에서 접근 가능한 클래스를 지칭한다. 만들어진 Singleton 인스턴스는 앱 전역에서 공유되기 때문에 앱의 전역 상태를 관리하기에 적합하다.

이 부분을 보면 강의에서 말한 한 번만 만들어진다고 했던 부분과 일치한다. 그렇다면 TSyringe에서 사용하는 @singleton 데코레이터도 위에 디자인 패턴을 사용하고 있는 것과 같다고 생각하면 될 것 같다.
