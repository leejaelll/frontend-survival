# Routers

### What are Routers?

- 모든 Express 애플리케이션에는 앱 라우터가 내장되어 있다.
- 라우터는 미들웨어 자체처럼 동작한다.
- app.use()의 인자로 사용하거나 다른 라우터의 use() 메서드에 인자로 사용할 수 있다.

애플리케이션을 만들 때 사용할 url 먼저 생각해보기

```text
/ -> Home
/join -> Join
/login -> Login
/search -> search
/edit-user -> Edit user
/delete-user -> Delete user
/watch-video -> Watch video
/edit-video -> Edit video
/delete-video -> Delete video
```

**라우터를 도메인별로 나누기 (user, video)**

```text
/ -> Home
/join -> Join
/login -> Login
/search -> search

/user/edit -> Edit user
/user/delete -> Delete user

/videos/watch -> Watch video
/videos/edit-> Edit video
/videos/delete -> Delete video
/videos/comments -> Comment video
/videos/comments/delete -> Delete a comment of a video
```

- 3개의 라우터가 존재
  1. 글로벌 라우터
  2. user 라우터
  3. video 라우터

---

### Making Our Routers

**라우터를 만드는 방법**

```js
const globalRouter = express.Router();
const userRouter = express.Router();
const videoRouter = express.Router();

app.use('/', globalRouter);
app.use('/users', userRouter);
app.use('/videos', videoRouter);

globalRouter.get('/', (req, res) => res.send('home'));
globalRouter.get('/join', (req, res) => res.send('join'));
globalRouter.get('/login', (req, res) => res.send('login'));
userRouter.get('/edit', (req, res) => res.send('Edit user'));
videoRouter.get('/watch', (req, res) => res.send('Watch video'));
```


---

### URL Parameters

**`:id`가 의미하는 것이 무엇일까?**
- parameter
- url에 변수 값을 넣어줄 수 있도록 도와준다. 
- express에게 변수라고 알려줄 수 있는 방법

 ```js
 videoRouter.get('/:id', (req, res) => {
    console.log(req.params)
    return res.send(`Watch video params ${req.params.id}`)
});
 ```

 <figure><img src="../../.gitbook/assets/230904-1.png" alt=""><figcaption></figcaption></figure>
 <figure><img src="../../.gitbook/assets/230904-2.png" alt=""><figcaption></figcaption></figure>

 - parameter 이름으로 작성한 id와 함께 값을 제공해준다. 



{% hint style="danger" %}
### parameter를 사용할 때 주의해야할 점

parameter를 써야한다면 parameter가 없는 url들을 먼저 작성한 후, 그 다음 parameter가 있는 url을 작성해야한다. 

만약 parameter가 있는 url이 먼저 온다면
```js
videoRouter.get('/:id', (req, res) => {
    console.log(req.params)
    return res.send(`Watch video params ${req.params.id}`)
});
videoRouter.get('/upload', (req, res) => res.send('upload video'));
```

`videos/upload`로 접근했을 때 respond를 받아오는 과정에서 /:id 의 변수 중 하나라고 인식한다.
{% endhint %}