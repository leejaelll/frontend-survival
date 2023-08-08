# 1⃣ 타입스크립트 알아보기

### 타입스크립트 설정 이해하기

tsconfig.json에서 compilerOptions을 설정할 수 있다. 그 중 `noImplicitAny`와 `strictNullChecks`를 이해해놓자.



#### **`noImplicitAny`**

타입을 명시할 수 있도록 도와주는 옵션. 명시되지 않은 타입을 타입스크립트가 추론할 때 any 로 추론하게 되면 컴파일 에러를 발생시킨다.&#x20;

```json
"noImplicitAny": true
```

<figure><img src="../../.gitbook/assets/스크린샷.png" alt=""><figcaption></figcaption></figure>

타입스크립트는 타입 정보를 가질 때 가장 효과적이기 때문에 되도록이면 noImplicitAny를 설정하고 시작하는 것이 좋다.



#### **`strictNullChecks`**

null과 undefined가 모든 타입에서 허용되는지 확인하는 설정이다.&#x20;

```json
"strictNullChecks": true
```



true로 설정되어 있는 경우, 명시된 타입과 다르게 null을 넣었을 때 에러를 발생시킨다. (undefined 동일)

<figure><img src="../../.gitbook/assets/스크린샷 (2).png" alt=""><figcaption></figcaption></figure>

argument가 number 또는 null이라면 명시적으로 드러내야한다.&#x20;

```typescript
const x: number | null = null;
```



{% hint style="info" %}

{% endhint %}
