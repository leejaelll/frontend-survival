# Can't reach database server at \`localhost\`:\`5432\`

## window

```properties
DATABASE_URL=postgresql://d2c:1234@localhost:5432/d2c?schema=public
```

pgadmin4를 사용하고 있다면, DATABASE\_URL에 해당하는 user와 database가 있는 지 확인해야 한다.&#x20;

추가로 database의 owner가 user의 이름과 같은지 확인해야 한다.&#x20;
