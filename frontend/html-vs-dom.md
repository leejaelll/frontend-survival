---
description: 속성(attribute)과 프로퍼티(property)는 근본적으로 다른 개념이다.
---

# HTML 속성 vs DOM 프로퍼티

## 핵심 차이점

### HTML 직렬화

* 속성은 HTML로 직렬화되지만, 프로퍼티는 그렇지 않다.&#x20;

```javascript
const div = document.createElement('div');

div.setAttribute('foo', 'bar');
div.hello = 'world';

console.log(div.outerHTML); // '<div foo="bar"></div>'
```

👉🏻 따라서 개발자 도구 요소 패널에선 요소의 속성만 확인할 수 있다.&#x20;



### 값(value)의 타입

* 속성 값은 항상 문자열이지만, 프로퍼티는 모든 타입이 가능하다.&#x20;

```javascript
const div = document.createElement('div');
const obj = { foo: 'bar' };

div.setAttribute('foo', obj);
console.log(typeof div.getAttribute('foo')); // 'string'
console.log(div.getAttribute('foo')); // '[object Object]'

div.hello = obj;
console.log(typeof div.hello); // 'object'
console.log(div.hello); // { foo: 'bar' }
```
