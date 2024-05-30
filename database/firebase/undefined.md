# 컬렉션 데이터 가져오기

## 컬렉션의 모든 데이터 가져오기

Firestore에 저장되어있는 `feeds` 컬렉션 내의 모든 문서를 가져오고 싶다고 가정해보자.&#x20;

Firestore에서 컬렉션의 모든 문서를 조회하려면? 👉🏻 **`getDocs`** 함수와 함께 컬렉션 참조를 사용해야 한다.

```javascript
import { collection, getDocs } from "firebase/firestore";
import { db } from "@/FirebaseApp";

export default async function getDataFeed() {
  const collectionRef = collection(db, "feeds"); //✅ 컬렉션 참조 생성
  const snapshot = await getDocs(collectionRef); //✅ 모든 문서 조회
  const documents = snapshot.docs.map((doc) => ({ 
    id: doc.id,
    ...doc.data(),
  }));
  console.log(documents); //✅ 조회된 문서의 데이터 출력
}
```

* `collection(db, "feeds")`를 호출하여 feeds 컬렉션에 대한 참조를 생성한다.&#x20;
* `getDocs(collectionRef)`를 통해 컬렉션의 모든 문서를 비동기적으로 조회한다. Promise를 반환하므로 await을 사용하여 처리한다.&#x20;
* 조회 결과인 snapshot 객체에서 .docs 배열을 통해 각 문서에 접근할 수 있다. 각 문서의 `.data()` 메서드로 데이터를 추출하고, .id 속성으로 문서의 ID를 가져온다.&#x20;



## 컬렉션 내 특정 문서 가져오기

Firestore에서 컬렉션의 특정 문서를 조회하려면? 👉🏻 **`getDoc`**&#x20;

```javascript
import { doc, getDoc } from "firebase/firestore";
import { db } from "@/FirebaseApp";

async function fetchDocument(id) {
  try {
    const docRef = doc(db, "drawings", id);
    const docSnap = await getDoc(docRef);

    if (docSnap.exists()) {
      console.log("Document data:", docSnap.data());
    } else {
      console.log("No such document!");
    }
  } catch (error) {
    console.error("Error fetching document:", error);
  }
}
```
