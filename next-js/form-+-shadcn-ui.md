---
description: react-hook-form과 shandcn/ui를 사용해 form 컴포넌트 구현하기
cover: >-
  https://images.unsplash.com/photo-1653103674098-6ed995323607?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTc0ODY5NTl8&ixlib=rb-4.0.3&q=85
coverY: 73.02749551703528
---

# Form 컴포넌트 구현하기 (+ shadcn/ui)

로그인 페이지를 구현한다고 가정해보자.&#x20;

<div align="left">

<figure><img src="../.gitbook/assets/240604-1 (1).png" alt="" width="375"><figcaption></figcaption></figure>

</div>

## 구현해야 할 기능

* 이메일, 패스워드 입력 👉🏻 input, form 컴포넌트로 구현, 유효성 검사
* 로그인 버튼 👉🏻 입력한 이메일과 패스워드를 서버에 전달 후 결과값 렌더링
* 회원가입 버튼 👉🏻 회원가입 페이지로 이동

***

## React Hook Form - shadcn/ui

Form의 기본적인 구조는 다음과 같다.&#x20;

```typescript
<Form>
  <FormField
    control={...}
    name="..."
    render={() => (
      <FormItem>
        <FormLabel />
        <FormControl>
          { /* Your form field */}
        </FormControl>
        <FormDescription />
        <FormMessage />
      </FormItem>
    )}
  />
</Form>
```

* Form
* FormField
* FormItem
* FormLabel
* FormControl
* FormDescription
* FormMessage

### formSchema 만들기

```typescript
"use client"

import { z } from "zod"

const formSchema = z.object({
  email: z.string().email({
    message: "유효하지 않은 이메일 주소입니다.",
  }),
  password: z.string().min(6, {
    message: "비밀번호는 최소 6자 이상이어야 합니다.",
  }),
});
```

### useform을 이용해 form 정의하기

```typescript
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import { signIn } from "next-auth/react";

type FormData = z.infer<typeof formSchema>;

export function Page() {
  // 1. Define your form.
  const form = useForm({
    resolver: zodResolver(formSchema),
    defaultValue: {
      email: "",
      password: "",
    },
  });

  // 2. Define a submit handler.
  const onSubmit = async (data: FormData) => {
    const { email, password } = data;

    const redirect_uri = (searchParmas.get("redirect_uri") as string) || undefined;
    const response: any = await singIn("credentials", {
      email,
      password,
      redirect: false,
      callbackUrl: redirect_uri,
    });

    if (!response.ok) {
      if (response.status === 401) {
        if (response.error === "USER_NOT_FOUND") {
          setOpenConfirmRegister(true); // AlertDialog 컴포넌트를 열고 닫기 위한 setState
          return;
        } else if (response.error === "PASSWORD_INCORRECT") {
          alert("비밀번호가 일치하지 않습니다.");
          return;
        }
      }

      alert("로그인에 실패했습니다. 다시 시도해주세요.");
      return;
    }
  };
}

```

### form 컴포넌트 작성하기

```typescript
<Form {...form}>
  <form onSubmit={form.handleSubmit(onSubmit)}>
    <FormField
      control={form.control}
      name="email"
      render={({ field }) => (
        <FormItem>
          <FormControl>
            <Input placeholder="이메일" {...field} type="email" />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
    <Button type="submit">
      로그인
    </Button>
  </form>
</Form>;

```



## 라우터 이동 핸들러 함수

```typescript
<div onClick={onRouteRegister}>또는 회원가입</div>
```

```typescript
const onRouteRegister = async (e: React.MouseEvent) => {
  const redirect_uri = (searchParams.get("redirect_uri") as string) || undefined;
  router.push(`/register/?email=${form.getValues("email")}&redirect_uri=${redirect_uri}`);
};
```

* 이메일 input에 값이 없다면 👉🏻 localhost:3000/register?email=\&redirect\_uri=undefined
* 이메일 input에 값이 있다면 👉🏻 localhost:3000/register?email=bienprltd@gmail.com\&redirect\_uri=undefined



## 회원정보가 없거나, 일치하지 않는 경우

### 회원정보가 없는 경우

onSubmit 함수 안에서 확인할 수 있다. 회원정보가 없는 경우, 아래 조건문에 걸리게 된다.&#x20;

```typescript
if (!response.ok) {
     if (response.status === 401) {
        if (response.error === "USER_NOT_FOUND") {
          setOpenConfirmRegister(true);
```

```typescript
const [openConfirmRegister, setOpenConfirmRegister] = useState(false);
```



shadcn/ui의  alertDialog 컴포넌트를 이용해 결과를 보여준다.&#x20;

```typescript
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from "@/components/ui/alert-dialog";

<AlertDialog open={openConfirmRegister} onOpenChange={setOpenConfirmRegister}>
  <AlertDialogContent>
    <AlertDialogHeader>
      <ExclamationIcon/>
      <AlertDialogTitle>가입되지 않은 이메일입니다.</AlertDialogTitle>
      <AlertDialogDescription>회원가입을 진행하시겠습니까?</AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>취소</AlertDialogCancel>
      <AlertDialogAction onClick={onRouteRegister}>회원가입</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

### 이메일, 패스워드가 일치하지 않는 경우

```typescript
else if (response.error === "PASSWORD_INCORRECT") {
  alert("비밀번호가 일치하지 않습니다.");
  return;
}
```



