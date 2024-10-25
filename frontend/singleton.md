# Singleton 패턴

그림판을 만들기 위해서 그림판 클래스 객체를 만들어야 한다고 가정할 때,&#x20;

```typescript
class Grimpan {
  contructor(canvas: HTMLElement | null) {
    if(!canvas || (canvas instance of HTMLCanvasElement)){
      throw new Error('canvas element 입력필요')
    }
  }
  initialize(){}
  initializeMenu(){}
}  
```



new Grimpan으로 클래스를 여러개 만들면 어떻게 될까?

```typescript
new Grimpan(document.querySelector('#canvas'))
new Grimpan(document.querySelector('#canvas'))
new Grimpan(document.querySelector('#canvas'))
new Grimpan(document.querySelector('#canvas'))
```

➡️ 하나의 객체에 여러 개의 클래스가 생성되면서 그림판 안에서 상태 공유가 불가능해진다.&#x20;



> ### _HTML 전체 객체에 대해서 그림판은 딱 한개만 존재해야 한다. 👉🏻 **Singleton Pattern**_



<mark style="background-color:orange;">**solution1**</mark>

instance 변수를 전역에 선언한다.&#x20;

```typescript
let instance:Grimpan;

class Grimpan {
  contructor(canvas: HTMLElement || null) {
    if(!canvas || (canvas instance of HTMLCanvasElement)){
      throw new Error('canvas element 입력필요')
    }
    
    if(!instance){
      instance = this;
    }
    
    return instance
  }
}  
```

➡️ instance와 클래스가 분리되어있는 것이 문제&#x20;



<mark style="background-color:orange;">**solution2**</mark>

Grimpan 클래스를 export default로 내보낸다.&#x20;

```typescript
class Grimpan {
  contructor(canvas: HTMLElement | null) {
    if(!canvas || (canvas instance of HTMLCanvasElement)){
      throw new Error('canvas element 입력필요')
    }
  }
  initialize(){}
  initializeMenu(){}
}  

export default new Grimpan(document.querySelector('#canvas'))
```

```typescript
import g1 from './grimpan.js';
import g2 from './grimpan.js';

console.log(g1 === g2) // true
```

자바스크립트 모듈의 특징 중 하나 ➡️ 모듈은 싱글턴이다. \
그렇기 때문에 다른 이름으로 그림판 클래스 객체를 가져와도 동일한 객체를 가져오는 것.



<mark style="background-color:orange;">**solution3**</mark>

클래스 내부에서 static 메서드를 사용한다.&#x20;

```typescript
class Grimpan {
  private instance:Grimpan;
  private contructor(canvas: HTMLElement | null) {
    if(!canvas || (canvas instance of HTMLCanvasElement)){
      throw new Error('canvas element 입력필요')
    }
  }
  initialize(){}
  initializeMenu(){}
  
  static getInstance(canvas: HTMLElemet | null) {
    if(!this.instance) {
      this.instance = new Grimpan(canvas)
    }
    
    return this.instance
  } 
}

Grimpan.getInstance()

exprot defualt Grimpan;
```

➡️ getInstance 메서드를 사용해서 객체를 가져올 경우 클래스 내부에 instance 선언이 가능하다. \
(단, instance도 static 이어야 한다.)



#### 싱글턴의 단점

* private: 테스트가 어렵다.
* getInstance: 단일책임원칙을 위반한다.&#x20;
