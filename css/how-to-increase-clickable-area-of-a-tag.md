---
cover: >-
  https://images.unsplash.com/photo-1705600958213-af097bb43791?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTE1MzMyNzF8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# a 태그 영역 설정

```html
<ul>
  <li><a href="/test">Example1</a></li>
  <li><a href="/test2">Example2</a></li>
  <li><a href="/test3">Example3</a></li>
</ul>
```

### CSS

```css
a {
  display: inline-block;
  padding: 5px 10px;
}
```

### `TailwindCSS`

```javascript
inline-block px-3 py-1
```

***

* li에 padding을 주는 것이 아닌 Link(a tag)에 padding 영역을 지정해준다.
* a 태그는 기본적으로 `display:inline`
* padding 영역을 주기 위해 display 속성을 inline-block 혹은 block으로 설정해준다.
