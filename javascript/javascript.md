# JavaScript로 재사용 컴포넌트 만들기

페이지의 header나 footer의 경우, 동일한 컴포넌트를 사용하게 된다.  vanillaJS로 재사용 컴포넌트를 어떻게 만들 수 있을까?&#x20;

## Custom Elements

<mark style="color:red;">새로운 HTML 요소를 정의</mark>하고, 해당 요소의 기능과 동작을 커스터마이즈할 수 있다. 👉🏻 이를 통해 <mark style="color:red;">재사용 가능한 컴포넌트</mark>를 만들고, 웹 애플리케이션의 유지보수성을 높일 수 있다.&#x20;



### Example

재사용할 컴포넌트를 외부 js파일에 작성한다.&#x20;

```javascript
class MyElement extends HTMLElement {
    constructor() {
        super();
        const shadow = this.attachShadow({ mode: 'open' });

        const wrapper = document.createElement('div');
        wrapper.textContent = "Hello, I am a custom element!";

        shadow.appendChild(wrapper);
    }
}

customElements.define('my-element', MyElement);
```

* MyElement 클래스는 HTMLElement를 상속받아 정의함
* `customElements.define` 메서드를 사용해 새로운 요소를 등록
* 이 요소는 Shadow DOM을 사용하여 내용을 캡슐화함



사용할 js파일에 정의한 이름으로 사용한다.&#x20;

```html
<!DOCTYPE html>
<html>
<head>
    <title>Custom Elements Example</title>
</head>
<body>
    <my-element></my-element>
    <script src="./my-element.js"></script>
</body>
</html>
```



## Custom Elements의 이점

• 재사용성: 특정 기능을 캡슐화한 컴포넌트를 만들어 재사용할 수 있다.

• 캡슐화: Shadow DOM을 사용하여 스타일과 동작을 캡슐화할 수 있다.

• 확장성: 기존 HTML 요소를 확장하거나 완전히 새로운 요소를 정의할 수 있다.

• 유지보수성: 컴포넌트 기반 아키텍처로 인해 코드의 유지보수성과 가독성이 향상된다.



***

## 참고

* [Custom elements](https://ko.javascript.info/custom-elements)
* [window: customElements property](https://developer.mozilla.org/en-US/docs/Web/API/Window/customElements)
