# PostgreSQL 설치, 실행

### postgresSQL install

postgresSQL 설치 (on Mac)

```bash
brew install postgresql
```

정상적으로 설치되었는지 확인하는 방법

```bash
postgres - V;
```

_➡️ postgres (PostgreSQL) 14.10 (Homebrew)_

<br />

### postgresSQL 실행

```bash
brew services start postgresql
```

<figure><img src="../../.gitbook/assets/240201-1.png" alt=""><figcaption></figcaption></figure>

default user로 postgre 접속

```bash
postgres-#: psql -U postgres
```

<figure><img src="../../.gitbook/assets/240201-2.png" alt=""><figcaption></figcaption></figure>

<br />

### 새로운 USER, Password 생성

```
postgres=# CREATE USER [myuser] WITH PASSWORD ['mypassword'];
CREATE ROLE
```

<br />

### USER NAME 변경

```
postgres=# ALTER USER bienpr RENAME TO postgres;
```

{% hint style="error" %}

_ERROR: session user cannot be renamed_

<figure><img src="../../.gitbook/assets/240201-8.png" alt=""><figcaption></figcaption></figure>

👉🏻 세션 사용자의 이름을 바꾸려고 할 때 오류가 발생한다. 다른 사용자로 로그인 한 후, 이름을 변경해야한다.

<figure><img src="../../.gitbook/assets/240201-9.png" alt=""><figcaption></figcaption></figure>

```
postgres=# ALTER USER test with superuser;
postgres=# ALTER USER test with createdb;
postgres=# ALTER USER test with createrole;
postgres=# ALTER USER test with replication;
```

<figure><img src="../../.gitbook/assets/240201-10.png" alt=""><figcaption></figcaption></figure>

```
postgres=# ALTER USER bienpr RENAME TO postgres;
ALTER ROLE
postgres=# DROP USER test;
DROP ROLE
```

{% endhint %}

<br />

### USER 삭제

```
postgres=# DROP USER [test];
DROP ROLE
```

<br />

### 현재 USER 조회

```
postgres=# select current_user;
```

<figure><img src="../../.gitbook/assets/240201-3.png" alt=""><figcaption></figcaption></figure>

<br  />

### 현재 선택된 database 조회

```
postgres=# select current_database();
```

<figure><img src="../../.gitbook/assets/240201-4.png" alt=""><figcaption></figcaption></figure>

---

### psql 명령어

- `\l` : 현재 서버에 있는 모든 데이터베이스 목록을 조회

  <figure><img src="../../.gitbook/assets/240201-5.png" alt=""><figcaption></figcaption></figure>

- `\c {database_name}` : 지정한 `database_name` 데이터베이스로 접속

  <figure><img src="../../.gitbook/assets/240201-6.png" alt=""><figcaption></figcaption></figure>

- `\du` : USER 조회

  <figure><img src="../../.gitbook/assets/240201-7.png" alt=""><figcaption></figcaption></figure>
