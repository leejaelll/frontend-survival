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


**<mark style="color:red;">logProduct는 비구조화 할당문을 사용해 구현하는 것이 낫다.</mark>**

- 비구조화 할당문은 모든 지역 변수의 타입이 추론되도록 한다. 
- 추가로 명시적 타입 구문을 넣는다면 불필요한 타입 선언으로 인해 코드가 번잡해진다. 
- 이상적인 타입스크립트 코드는 함수/메서드 시그니처에 타입 구문을 포함하지만, 함수 내에서 생성된 지역 변수에는 타입 구문을 넣지 않는다. 

<br>

**함수 매개변수에 타입 구문을 생략하는 경우도 간혹 있다.**
```typescript
function parseNumber(str: string, base= 10) {
    //...
}
```
- base의 기본값이 10이기 때문에 base의 타입은 number로 추론된다. 
- 보통 타입 정보가 있는 라이브러리에서, 콜백 함수의 매개변수 타입은 자동으로 추론된다. 
    - express HTTP 서버 라이브러리를 사용하는 reqeust, response의 타입 선언은 필요하지 않다. 
    ```typescript
    // 이렇게 하지 맙시다. 
    app.get('/health', (request: express.Request, response: express.Response) => {
        repsonse.send('OK');
    })

    // 이렇게 합시다. 
    app.get('health', (request, response) => {
        reponse.send('OK');
    })
    ```


**타입이 추론될 수 있음에도 여전히 타입을 명시하고 싶은 상황이 있다. 👉🏻 객체 리터럴을 정의할 때**
```typescript
const elmo: Product = {
    name: 'Tickle Me Elmo',
    id: '048188',
    price: 28.99,
}
```
- 이런 정의에 타입을 명시하려면 잉여 속성 체크가 동작한다. 
    - 잉여 속성 체크는 선택적 속성이 있는 타입의 오타 같은 오류를 잡는데 효과적
    - 변수가 사용되는 시점이 아닌 할당하는 시점에 오류가 표시되도록 해준다. 
- 만약 타입 구문을 제거한다면? 잉여 속성 체크가 동작하지 않고, 객체를 선언한 곳이 아니라 사용되는 곳에서 타입 오류가 발생한다.
    ```typescript
    logProduct(furby);
    // 형식의 인수는 'Product' 형식의 매개변수에 할당할 수 없습니다. 
    // id 속성의 형식이 호환되지 않습니다. 
    // number 형식은 'string' 형식에 할당할 수 없습니다. 
    ```
- 타입 구문을 제대로 명시한다면 실제로 **실수가 발생한 부분**에 오류를 표시해준다. 
    ```typescript
    const furby: Product = {
        name: 'Furby',
        id: 630509430963,
        // 'number 형식은 'string' 형식에 할당할 수 없습니다. 
        price: 35,
    };
    logProduct(furby);
    ```

함수의 반환에도 타입을 명시하여 오류를 방지할 수 있다. 
- 타입 추론이 가능할지라도 구현상의 오류가 함수를 호출한 곳까지 영향을 미치지 않도록 하기 위해 타입 구문을 명시하는 것이 좋다. 
- 주식 시세롤 조회하는 함수를 작성했다고 가정해보자. 
    ```typescript
    function getQuote(ticker: string) {
        return fetch(`https://quotes.example.com/?q=${ticker}`).then(response => response.json());
    }
    ```
- 이미 조회한 종목을 다시 용청하지 않도록 캐시를 추가한다. 
    ```typescript
    const cache: {[ticker: string] : number} = {};
    function getQuote(ticker: string) {
        if(ticker in cache) {
            return cache[ticker];
        }
        return return fetch(`https://quotes.example.com/?q=${ticker}`).then(response => response.json).then(quote => {
            cache[ticker] = quote;
            return quote;
        })
    }
    ```
    - 이 코드의 오류: getQuote는 항상 Promise를 반환하므로 if 구문에는 `cache[ticker]`가 아니라 `Promise.resolve(cache[ticker])`가 반환되도록 해야한다.
    - 실행해보면 오류는 getQuote 내부가 아닌 getQuote를 호출한 코드에서 발생한다. 
        ```typescript
        getQuote('MSFT).then(considerBuying); 
        // 'number | Promise<any>' 형식에 'then' 속성이 없습니다. 
        // 'number' 형식에 'then' 속성이 없습니다. 
        ```
    - 이때, 의도된 반환 타입(Promise<any>)를 명시한다면 정확한 위치에 오류가 표시된다. 
        ```typescript
        const cache: {[ticker: string] : number} = {};
        function getQuote(ticker: string): Promise<number> {
            if(ticker in cache) {
                return cache[ticker];
                // 'number'형식은 'Promise<any> 형식에 할당할 수 없습니다.'
            }
        }
        ```
    

{% hint style="danger" %}
### 반환 타입을 명시해야 하는 또 다른 이유

오류의 위치를 제대로 표시해주는 이점 외에도 반환 타입을 명시해야하는 두 가지 이유가 더 있다. 

1. 반환 타입을 명시하면 함수에 대해 더욱 명확하게 알 수 있기 때문이다. 
    - 반환 타입을 명시하려면 구현하기 전에 입력 타입과 출력 타입이 무엇인지 알아야 한다. 
    - 추후에 코드가 변경되어도 그 함수의 시그니처는 쉽게 바뀌지 않는다. 
2. 명명된 타입을 사용하기 위해서
    - 반환 타입을 명시하면 더욱 직관적인 표현이 된다. 
    - 반환 값을 별도의 타입으로 정의하면 타입에 대한 주석을 작성할 수 있어서 자세한 설명이 가능하다.
{% endhint %}

**요약**
- 타입스크립트가 타입을 추론할 수 있다면 타입 구문을 작성하지 않는게 좋다. 
- 함수/메서드의 시그니처에는 타입 구문이 있지만, 함수 내의 지역 번수에는 타입구문이 없다. 
- 추론될 수 있는 경우라도 객체 리터럴과 함수 반환에는 타입 명시를 고려해야 한다. 

---

### 다른 타입에는 다른 변수 사용하기

타입스크립트는 한 변수에 다른 목적을 가지는 다른 타입으로 재사용하는 것을 막는다. 👉🏻 변수의 값은 바뀔 수 있지만 타입은 바꿀 수 없다. 

**타입을 바꿀 수 있는 한 가지 방법**
- 범위를 좁히는 것
- 새로운 변수 값을 포함하도록 확장하는 것이 아니라 **타입을 더 작게 제한하는 것**

오류가 발생하는 타입스크립트 코드
```typescript
let id = "12-34-56";
id = 123456;
```
- id 타입을 바꾸지 않고 해결하려면? 👉🏻 string과 number를 모두 포함할 수 있는 유니온 타입으로 선언
    ```typescript
    let id : string|number = "12-34-56";
    id = 123456;
    ```
- 유니온 타입으로 코드가 동작은 할 수 있지만, id를 사용할 때마다 값이 어떤 타입인지 확인해야 하기 때문에 차라리 별도의 변수를 도입하는 것이 낫다. 


**<mark style="color:red;">다른 타입에는 다른 변수를 사용하는게 바람직한 이유</mark>**

1. 서로 관련이 없는 두 개의 값을 분리할 수 있다. 
2. 변수명을 구체적으로 지을 수 있다. 
3. 타입 추론을 향상시키며, 타입 구문이 불필요해진다. 
4. 타입이 좀 더 간결해진다. 
5. let 대신 const로 변수를 선언하게 된다. 

---

### 타입 넓히기

> 런타임에 모든 변수는 유일한 값을 가진다. 그러나 타입스크립트가 작성된 코드를 체크하는 정적 분석 시점에, 변수는 '가능한' 값들의 집합인 타입을 가진다. 상수를 사용해서 변수를 초기화할 때 타입을 명시하지 않으면 타입 체커는 타입을 결정해야한다. 이 말은 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추해야 한다는 뜻이다. 타입스크립트에서는 이러한 과정을 '넓히기(widening)'이라고 부른다.


벡터를 다루는 라이브러리를 작성한다고 가정해보자. 
```typescript
interface Vector3 {
    x: number;
    y: number;
    z: number;
}

function getComponent(vector: Vector3, axis: 'x'|'y'|'z') {
    return vector[axis]
}
```

Vector3 함수를 사용한 코드는 런타임에 오류없이 실행되지만, 편집기에서는 오류가 표시된다. 

```typescript
let x = 'x';
let vec = {x: 10, y: 20, z: 30};
getComponent(vec, x);
// 'string' 형식의 인수는 "x" | "y" | "z" 형식의 매개변수에 할당될 수 없습니다. 
```

- getComponent 함수는 두 번째 매개변수에 "x" | "y" | "z" 타입을 기대했지만, x의 타입은 할당 시점에 넓히기가 동작해서 string으로 추론되었다. 
- string 타입은 "x" | "y" | "z" 타입에 할당이 불가능하기 때문에 오류를 발생시킨다. 


{% hint style="info" %}
### 타입스크립트에서 제공하는 넓히기 과정을 제어할 수 있는 방법

const로 선언하기
- let 대신 const로 선언하면 더 좁은 타입이 된다. 
- 위 예제에서 x는 재할당될 수 없으므로 의심의 여지 없이 좁은 타입으로 추론할 수 있다. 

_const가 만능은 아니다._ 
객체의 배열의 경우에는 여전히 문제가 있다. 

```typescript
const mixed = ['x', 1];
```
- 위 코드는 튜플을 추론해야 할지, 요소들은 어떤 타입으로 추론해야 할지 알 수 없다.

```typescript
const v = {
    x: 1,
}

v.x = 3;
v.x = '3';
v.y = 4;
v.y = 'Pythagoras';
```
- v타입은 구체적인 정도에 따라 다양한 모습으로 추론될 수 있다. 
- 객체의 경우, 타입스크리브의 넓히기 알고리즘은 각 요소를 let으로 할당된 것처럼 다룬다. 그렇기 때문에 v 타입은 {x: number}가 된다.
{% endhint %}

<br>

타입 추론의 강도를 직접 제어하려면 타입스크립트의 기본 동작을 재정의해야한다. 

**타입스크립트의 기본 동작을 재정의하는 세 가지 방법**
1. 명시적 타입 구문 제공
    ```typescript
    const v: {x: 1|3|5} = {
        x: 1 // 타입이 {x: 1|3|5}
    }
    ```
2. 타입 체커에 추가적인 문맥 제공
    - 예를 들어, 함수의 매개변수로 값을 전달
3. const 단언문을 사용하는 것
    ```typescript
    const v1 = {
        x: 1,
        y: 2
    }; // 타입은 { x: number; y: number; }

    const v2 = {
        x: 1 as const,
        y: 2
    }; // 타입은 { x: 1, y: number; }

    const v3 = {
        x: 1, 
        y: 2,
    } as const; // 타입은 { readonly x: 1; readonly y: 2; }
    ```
    - 값 뒤에 as const를 작성하면 타입스크립트는 최대한 좁은 타입으로 추론한다. 
    - v3이 진짜 상수라면, 주석에 보이는 추론된 타입이 실제로 원하는 형태일 것이다. 

    
_<mark style="color:red;">**넓히기로 인해 오류가 발생한다고 생각되면, 명시적 타입 구문 또는 const 단언문을 추가하는 것을 고려해야 한다. **</mark>_

**요약**
- 타입스크립트가 넓히기를 통해 상수의 타입을 추론하는 법을 이해해야 한다. 
- 동작에 영향을 줄 수 있는 방법인 const, 타입 구문, 문맥, as const에 익숙해져야 한다. 

---

### 타입 좁히기

> 타입 좁히기는 타입스크립트가 넓은 타입으로부터 좁은 타입으로 진행하는 과정을 의미한다. 

**대표적인 예시 👉🏻 null 체크**

```typescript
const el = document.getElementById('foo'); // 타입이 HTMLElement | null
if(el) {
    el // 타입이 HTMLElement
    el.innerHTML = 'Party Time'.blink()
} else {
    el // 타입이 null
    alert('No element #foo') 
}
```

타입을 좁히는 방법
1. 조건문 분기처리
    ```typescript
    const el = document.getElementById('foo');
    if(!el) throw new Error('Unable to find #foo')
    el;
    el.innerHTML = 'Party Time'.blink()
    ```
2. instanceof 사용
    ```typescript
    function contains(text: string, search: string | RegExp) {
        if(search instanceof RegExp) {
            search
            return !!search.exec(text)
        } 
        search
        return text.includes(search)
    }
    ```
3. 속성체크
    ```typescript
    interface A { a: number }
    interface B { b: number }
    function pickAB(ab: A | B) {
        if('a' in ab) {
            ab // 타입이 A
        } else {
            ab // 타입이 B
        }
        ab // 타입이 A | B
    }
    ```
4. Array.isArray와 같은 내장 함수 사용
    ```typescript
    function contains(text: string, terms: string|string[]) {
        const termList = Array.isArray(terms) ? terms: [terms];
        termList // 타입이 string[]
        // ...

    }
    ```