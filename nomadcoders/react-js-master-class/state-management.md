# State Management

### Recoil을 이용해 상태 관리하기

✅ TODO 리스트 만들기

**atom 만들기**
todo를 상태로 관리하기 위해선 atom을 만들어야 한다. 

```typescript
import { atom } from "recoil";

export const toDoState = atom({
  key: "toDo",
  default: [],
})
```

toDo의 형태는 객체로 text, id, category를 가진다. 
```typescript
export interface IToDo {
  text: string;
  id: number;
  category: "TO_DO" | "DOING" | "DONE";
}

export const toDoState = atom<IToDo>({
  key: "toDo",
  default: [],
})
```


### React Hook Form을 이용해 TextField 컴포넌트 만들기

TextField 컴포넌트에선 input에 입력한 텍스트를 toDo 상태로 만들고 배열에 추가시켜야한다. 

상태 값을 변경하는 함수를 사용하기 위해서는 useSetRecoilState를 사용한다. 

```typescript
import { useForm } from "react-hook-form";
import { useSetRecoilState } from "recoil";
import { toDoState } from "../atoms";

type IForm = {
  toDo: string;
}

export default function CreateToDo() {
  const setToDos = useSetRecoilState(toDoState); // setterFn을 사용할 수 있다. 
  const { register, handleSubmit, setValue } = useForm<IForm>(); 

  // handleValid 함수는 새로운 todo를 받아 기존 todo 배열에 추가해야한다. 
  const handleValid = ({ toDo }: IForm) => {
    setToDos(oldToDos => [{ text: toDo, id: Date.now(), category: "TO_DO" }, ...oldToDos])
    setValue("toDo", "");
  }


  return (
     <form onSubmit={handleSubmit(handleValid)}>
      <input
        {...register("toDo", {
          required: "Please write a To Do",
        })}
        placeholder="Write a to do"
      />
      <button>Add</button>
    </form>
  )
}

```