# URL 쿼리 파라미터 객체로 변환하기 👉🏻 Object.fromEntries()

## `Object.fromEntries()`

```javascript
const params = new URLSearchParams('user=John&age=30');
const obj = Object.fromEntries(params);
console.log(obj);
// 출력: { user: 'John', age: '30' }
```

* 배열이나 맵(Map)과 같이 \[key, value] 형태의 키-값 쌍을 포함하는 반복 가능한 객체를 입력 받아 새로운 객체를 생성한다.&#x20;



