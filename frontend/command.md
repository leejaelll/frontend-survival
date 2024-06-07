# Command 패턴

> 커맨드 패턴을 사용하면 **특정 작업을 실행하는 개체**와 **메서드를 호출하는 개체**를 분리할 수 있다.&#x20;

## Command 패턴

_<mark style="color:red;">**명령을 처리하는 객체를 통해 메서드와 실행되는 동작의 결합도를 낮출 수 있다.**</mark>_&#x20;



온라인 음식 배달 플랫폼을 개발한다고 가정해 보자.\
사용자는 주문하거나, 주문한 음식이 어디쯤 왔는지 확인하거나, 주문을 취소할 수 있다.&#x20;

```jsx
class OrderManager() {
  constructor() {
    this.orders = []
  }
  
  placeOrder(order,id) {
    this.order.push(id)
    return `You have successfully ordered ${order} (${id})`;
  }
  
  trackOrder(id) {
    return `Your order ${id} will arrive in 20 minutes.`
  }
  
  cancelOrder(id) {
    this.orders = this.orders.filter(order => order.id !== id)
    return `You have canceled your order ${id}`
  }
}
```

* `OrderManager`클래스에서 `placeOrder`, `trackOrder`, `cancelOrder`를 구현하였고. 이 메서드들을 직접 사용하는것도 가능하다.

```jsx
const manager = new OrderManager()

manager.placeOrder('Pad Thai', '1234')
manager.trackOrder('1234')
manager.cancelOrder('1234')
```

👉🏻 이렇게 manager 메서드를 직접 사용하는 도중, 나중에 특정 메서드의 이름을 변경하거나 메서드의 기능을 변경해야 하는 경우가 생길 수 있다. `placeOrder`대신에 이름을 `addOrder`로 변경하면 전체 코드베이스에서 해당 메서드를 호출하지 않도록 코드를 수정해야 한다. 앱의 규모가 크면 까다로운 작업이 될 것이다.



### 메서드 분리하기

위와 같은 코드처럼 작성하는 대신, manager 객체로부터 메서드를 분리하고 각각의 명령을 처리하는 함수를 만든다.&#x20;

#### placeOrder 리팩토링

`placeOrder`, `trackOrder`, `cancelOrder`를 직집 구현하는 대신 `execute`라는 하나의 메서드만 가지도록 한다. 이 메서드는 인자로 주어진 어떤 명령이든 실행할 수 있다.

각 명령은 첫 번째 인자로 `OrderManager`의 `orders` 배열을 넘겨 접근할 수 있도록 한다.

```jsx
class OrderManager {
  constructor() {
    this.orders = []
  }
  
  execute(command, ...args) {
    return command.execute(this.orders, ...args)
}
```

\


아래에서 3개의 `Command` 를 만든다.

* `PlaceOrderCommand`
* `CancelOrderCommand`
* `TrackOrderCommand`

```jsx
class Command {
  constructor(execute) {
    this.execute = execute
  }
}

function PlaceOrderCommand(order, id) {
  return new Command(orders => {
    orders.push(id)
    return `You have successfully ordered ${order} (${id})`
  })
}

function CancelOrderCommand(id) {
  return new Command(orders => {
    orders = orders.filter(order => order.id !== id)
    return `You have canceled your order ${id}`
  })
}

function TrackOrderCommand(id) {
  return new Command(() => `Your order ${id} will arrive in 20 minutes.`)
}

const manager = new OrderManager();

manager.execute(new PlaceOrderCommand("Pad Thai", "1234"));
manager.execute(new TrackOrderCommand("1234"));
manager.execute(new CancelOrderCommand("1234"));
```

***

## 장점 <a href="#undefined" id="undefined"></a>

커멘드 패턴은 객체와 메서드를 분리할 수 있게 해 준다. 이렇게 분리하면 수명이 지정된 명령을 만들거나. 명령들을 큐에 담아 특정한 시간대에 처리하는 것도 가능해진다.

## 단점 <a href="#undefined" id="undefined"></a>

커멘드 패턴을 쓸만한 상황이 딱히 많지 않고 종종 불필요한 코드가 만들어지곤 한다.

## 참조

[https://patterns-dev-kr.github.io/design-patterns/command-pattern/](https://patterns-dev-kr.github.io/design-patterns/command-pattern/)
