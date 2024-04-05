# pgvector

### install vector extension

{% hint style="danger" %}
_**ERROR: Unique constraint failed on the fields: (****`userId`****)**_

could not open extension control file "/opt/homebrew/share/postgresql@14/extension/vector.control": No such file or directory

👉🏻 vector extension 기능이 설치되어있지 않을 때 발생
{% endhint %}

[pgvector 공식문서](https://github.com/pgvector/pgvector)&#x20;

```bash
cd /tmp
git clone --branch v0.6.2 https://github.com/pgvector/pgvector.git
cd pgvector
make
make install # may need sudo
```



postgres로 접속한 후, vector extension을 생성한다.&#x20;

```bash
psql -U postgres

CREATE EXTENSION vector;
```



{% hint style="warning" %}
_**ERROR: permission denied to create extension "vector"**_\
\
👉🏻 vector를 만들 권한이 없는 user일 경우 발생
{% endhint %}

<figure><img src="../../.gitbook/assets/240401-1.png" alt=""><figcaption></figcaption></figure>

user role에 `Superuser`가 있는지 확인 후, 없다면 `ALTER USER myuser with superuser;` 실행 ➡️ 권한 등록&#x20;
