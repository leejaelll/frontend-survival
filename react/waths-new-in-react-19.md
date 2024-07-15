---
cover: >-
  https://images.unsplash.com/photo-1720312550294-db7104f96a6c?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MjEwMjEwNzJ8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Wath's new in React 19



## Actions

데이터 변경이 되면 응답에 따라 상태를 변경해야한다. 예를 들어, 유저가 이름을 변경했을 때 API 요청을 하고 응답을 핸들링한다. 과거에는 pending 상태, 에러, 업데이트, 순차적 요청을 관리했다.&#x20;

`useState`로 에러 상태를 관리했다면:

```typescript
// Before Actions
function UpdateName({}) {
  const [name, setName] = useState('');
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);

  const handleSubmit = async () => {
    setIsPending(true);
    const error = await updateName(name);
    setIsPending(false);
    if (error) {
      setError(error);
      return;
    }
    redirect('/path');
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```



React 19에서는 트랜지션에서 비동기 함수를 사용하여 보류 상태, 오류, 양식 및 낙관적 업데이트를 자동으로 처리하는 기능이 추가된다.&#x20;

```javascript
function UpdateName({}) {
  const [name, setName] = useState('');
  const [error, setError] = useState(null);
  // const [isPending, setIsPending] = useState(false);
  const [ispending, setIsPending] = useTransition();

  const handleSubmit = () => {
    startTransition(async () => {
      const error = await updateName(name);
      if (error) {
        setError(error);
        return;
      }
      redirect('/path');
    });
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.name)} />
      <button onClick={handleSubmit} disabled={ispending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```



{% hint style="info" %}
#### 비동기 전환을 사용하는 함수를 "Actions"라고 한다.&#x20;

액션은 자동으로 데이터 제출을 관리한다.

* Pending state: 액션은 요청이 시작될 때 시작되어 최종 상태 업데이트가 커밋되면 자동으로 재설정되는 보류상태를 제공한다.&#x20;
* Optimistic updates: 요청이 제출되는 동안 사용자에게 즉각적인 피드백을 표시할 수 있도록 `useOptimistic` 을 지원한다.&#x20;
* Error handling: 오류 처리 기능을 제공하므로 에러 바운더리를 표시하고 원래 값으로 되돌릴 수 있다.
* Forms: `<form>` 요소는 action과 formAction 프로퍼티에 함수를 전달할 수 있다.
{% endhint %}





### useActionState

* 액션의 일반적인 경우를 더 쉽게 처리할 수 있다.

```javascript
// Using <form> Actions and useActionState
function ChangeName({ name, setName }) {
  const [error, submitAction, isPending] = useActionState(async (previousState, formData) => {
    const error = await updateName(formData.get('name'));
    // actions의 결과를 리턴한다.
    // 에러를 리턴한다.
    if (error) {
      return error;
    }
    // 성공했을 때
    redirect('/path');
    return null;
  }, null);

  return (
    <form action={submitAction}>
      <input type='text' name='name' />
      <button type='submit' disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

* 래핑된 액션이 호출되면, useActionState는 액션의 마지막 결과를 데이터로 반환하고 액션의 보류 상태를 보류 중으로 반환한다.&#x20;



### useFormStatus

* 디자인시스템에서는 form에 대한 정보에 접근해야할 때, props를 전달하지 않고 접근할 수 있도록 작성하는 것이 일반적이다.&#x20;
* 이 작업은 컨텍스트를 통해 수행할 수도 있지만, 일반적인 경우 더 쉽게 만들기 위해 useFormStatus를 추가함.

```javascript
import {useFormStatus} from 'react-dom';

function DesignButton() {
  const {pending} = useFormStatus();
  return <button type="submit" disabled={pending} />
}
```

* Context Provider처럼 부모 form 컴포넌트의 상태를 읽는다.&#x20;



### useOptimistic

* 데이터 변형을 수행할 때 비동기 요청이 진행되는 동안 상태를 표시하는 것이 일반적

```javascript
function ChangeName({ name, setName }) {
  const [optimisticName, setOptimisticName] = useOptimistic(currentName);

  const submitAction = async (formData) => {
    const newName = formData.get('name');
    setOptimisticName(newName);
    const updatedName = await updateName(newName);
    onUpdateName(updatedName);
  };

  return (
    <form action={submitAction}>
      <p>Your name is: {optimisticName}</p>
      <p>
        <label>Change Name:</label>
        <input type='text' name='name' disabled={currentName !== optimisticName} />
      </p>
    </form>
  );
}

```

* updateName 요청이 진행되는 동안 optimiticName을 즉시 렌더링한다.&#x20;
* 업데이트가 완료되거나 오류가 발생하면 currentName 값으로 전환한다.&#x20;



### use

```javascript
import {use} from 'react';

function Comments({commentsPromise}) {
  // `use` will suspend until the promise resolves.
  const comments = use(commentsPromise);
  return comments.map(comment => <p key={comment.id}>{comment}</p>);
}

function Page({commentsPromise}) {
  // When `use` suspends in Comments,
  // this Suspense boundary will be shown.
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Comments commentsPromise={commentsPromise} />
    </Suspense>
  )
}
```

* early return 과 같이 조건부로 context를 읽을 때 use를 사용할 수 있다.&#x20;

```javascript
import {use} from 'react';
import ThemeContext from './ThemeContext'

function Heading({children}) {
  if (children == null) {
    return null;
  }
  
  // This would not work with useContext
  // because of the early return.
  const theme = use(ThemeContext);
  return (
    <h1 style={{color: theme.color}}>
      {children}
    </h1>
  );
}
```

* use는 hooks와 마찬가지로 렌더링될 때만 호출할 수 있다.&#x20;
* 하지만 조건부로 호출할 수 있다는 점이 특징







***



#### Reference

* [https://react.dev/blog/2024/04/25/react-19](https://react.dev/blog/2024/04/25/react-19)

