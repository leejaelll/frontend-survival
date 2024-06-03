# 두 배열의 겹치는 값 찾기 👉🏻 Set

```javascript
const a = [1, 3, 5, 5, 7, 0]
const b = [3, 5, 5, 6, 7, 8] 
```

두 배열에서 겹치는 값만 찾으려면?

#### 첫 번째 방법 - for문

```javascript
const intersection = [];
for(const v of a) {
    if(b.includes(v)) {
        intersection.push(v)
    }
}
```



#### 두 번째 방법 - filter 함수

```javascript
a.filter(v => b.includes(v));
```



#### 세 번째 방법 - filter 함수 & Set 객체

```java
a.filter(v => new Set(b).has(v));

// 가장 효율적인 방법
a.filter(Set.prototype.has, new Set(b)); // [3, 5, 5, 7]
```



만약 겹치는 값 중에서도 중복된 값은 제거하려면? 👉🏻 Set의 intersection 메서드를 사용한다. /
