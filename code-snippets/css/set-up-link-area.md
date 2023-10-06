# a 태그 영역 설정

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

### `tailwindCSS`

```typescript
<li><Link href="/" className="inline-block px-3 py-1">Example1<Link></li>
```

---

- li에 padding을 주는 것이 아닌 Link(a tag)에 padding 영역을 지정해준다.
- a 태그는 기본적으로 `display:inline`이다.
- padding 영역을 주기 위해 display 속성을 inline-block 혹은 block으로 설정해준다.
