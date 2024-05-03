# Not Found Page 만들기

{% code title="app/products/[productId]/reviews/[reviewId]/not-found.tsx.tsx" %}
```typescript
export default function NotFound() {
    return (
        <div>
            <h2>Page not found</h2>
            <p>Could not find requested resource</p>
        </div>
    );
}
```
{% endcode %}



#### NotFound component 렌더링하는 방법

실행하고자하는 컴포넌트에서 next/navigation 'notFound' 함수를 불러온다.&#x20;

```typescript
import { notFound } from "next/nevigation";

export default function ReviewDetail({
    params,
}: {
    params: {
        productId: string;
        reviewId: string;
    }
    if(parseInt(params.reviewId) > 100) {
        notFound();
    }
}) {
    return (
        <h1> Review {params.reviewId} for product {params.productId}</h1>
    );
}
        
```
