# client side에서 캐시를 효율적으로 관리하는 방법 👉🏻 staleTimes

### `staleTimes`

클라이언트 측 라우터 캐시에서 페이지 세그먼트를 캐싱할 수 있도록 하는 기능

{% code title="next.config.js" %}
```typescript

const nextConfig={
  experimental: {
    staleTimes: {
      dynamic:30,
      static: 180,
    }
  }
}
```
{% endcode %}

* dynamic
  * 페이지가 정적으로 생성되지도 않고 완전히 prefetch되지 않을 때 사용
  * 기본 값은 0초
* static
  * 정적으로 생성된 페이지에 사용되거나 링크의 prefetch 프로퍼티가 true로 설정된 경우
  * router.prefetch()를 호출할 때 사용
