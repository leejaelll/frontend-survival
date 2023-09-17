# 타입 추론

### 추론 가능한 타입을 사용해 장황한 코드 방지하기

모든 변수에 타입을 선언하는 것은 비생산적이다. 

```typescript
const person : {
    name: string;
    born: {
        where: string;
        when: string;
    }
    died: {
        where: string;
        when: string;
    }
} = {
    name: 'Sojourner Truth',
    born: {
        where: 'Swatekill, NY',
        when: 'c.1797',
    },
    died: {
        where: 'Battle Creek, MI',
        when: 'Nov. 26, 1883'
    }
}
```

👉🏻 타입을 생략하고 작성해도 충분하다. 

```typescript
const person = {
    name: 'Sojourner Truth',
    born: {
        where: 'Swatekill, NY',
        when: 'c.1797',
    },
    died: {
        where: 'Battle Creek, MI',
        when: 'Nov. 26, 1883'
    }
}
```

타입스크립트는 예상한 것보다 더 정확하게 추론하기도 한다. 
```typescript
const axis1: string = 'x' // 타입은 string
const axis2 = 'y' // 타입은 'y'
```
👉🏻 axis2 타입이 string으로 예상하기 쉽지만 타입스크립트가 추론한 'y'가 더 정확한 타입이다. 

<br>
타입이 추론되면 리팩터링 역시 용이해진다. 

product 타입과 기록을 위한 함수를 가정해보자.
```typescript
interface Product {
    id: number;
    name: string;
    price: number;
}

function logProduct(product: Product) {
    const id: number = product.id;
    const name: string = product.name;
    const price: number = product.price;
    console.log(id, name, price);
}
```

- id에 문자도 들어있을 수 있음을 나중에 알게된다면? Prodcut 내의 id 타입을 변경한다. 
    ```typescript
    interface Product {
        id: string;
        name: string;
        price: number;
    }

    function logProduct(product: Product) {
        const id: number = product.id; // ~~ 'string' 형식은 'number' 형식에 할당할 수 없습니다. 
        const name: string = product.name;
        const price: number = product.price;
        console.log(id, name, price);
    }
    ```
- 그러면 logProduct 내의 id 변수 선언에 있는 타입과 맞지 않기 떄문에 오류가 발생한다. 
- logProduct 함수 내의 명시적 타입 구문이 없었다면, 코드는 아무런 수정 없이도 타입 체커를 통과했을 것.


<mark style="color:red;">**logProduct는 비구조화 할당문을 사용해 구현하는 것이 낫다.**</mark>

- 비구조화 할당문은 모든 지역 변수의 타입이 추론되도록 한다. 
- 추가로 명시적 타입 구문을 넣는다면 불필요한 타입 선언으로 인해 코드가 번잡해진다. 

