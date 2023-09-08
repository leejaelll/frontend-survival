# MongoDB and Mongoose

### Introduction to MongoDB

**MongoDB**
- document-oriented database, 즉 문서 지향 데이터베이스의 범주에 속한다. 
- JSON(JavaScript Object Notation) 문서와 유사한 형식으로 데이터를 저장한다는 의미

**Install MongoDB**
- [Install MongoDB Community Edition](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/#install-mongodb-community-edition)

```
$ xcode-select --install
$ brew tap mongodb/brew
$ brew install mongodb-community@7.0

```

---

### Connecting to Mongo

**Mongoose**
- MongoDB와 함께 사용되는 Node.js 기반의 ODM (Object Data Modeling) 라이브러리
- Node.js와 MongoDB를 이어주는 역할을 한다.
- 자바스크립트로 코드를 적으면 mongoose가 mongoDB에게 전달한다.

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://127.0.0.1:27017/test');

const Cat = mongoose.model('Cat', { name: String });

const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
```


**Install Mongoose**
```
$ npm install mongoose --save

```

**mongoose 세팅하기**
database.js 파일 생성
```js
import mongoose from 'mongoose';

mongoose.connect('mongodb://127.0.0.1:27017/youtube');
```
- database.js 파일을 server.js에서 import해줌으로써, monogoDB에 연결

```js
import mongoose from 'mongoose';

mongoose.connect('mongodb://127.0.0.1:27017/youtube', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const db = mongoose.connection;

const handleOpen = () => console.log('✅ Connected to DB');
const handleError = (error) => console.log('❌ DB Error', error);

db.on('error', handleError);
db.once('open', handleOpen);

```

{% hint style="info" %}
### on과 once의 차이점
- on은 여러 번 발생시킬 수 있음.
- once는 한 번만 발생함
{% endhint %}

<figure><img src="../../.gitbook/assets/230906-2.png" alt=""><figcaption></figcaption></figure>

---

### CRUD Introduction

**CRUD**
: create, read, update, delete


mongoose는 mongoDB와 대화할 수 있도록 해준다. 그러기 위해선 mongoose를 도와줄 필요가 있다. 👉🏻 데이터가 어떻게 생겼는지 알려줘야 한다.


**`new mongoose.Schema`**
: Schema를 정의할 수 있다. 

```js
const videoSchema = new mongoose.Schema({
  title: String,
  description: String,
  createdAt: Date,
  hashtags: [{ type: String }],
  meta: {
    views: Number,
    rating: Number,
  },
});
```

**`mongoose.model('Video', videoSchema)`**