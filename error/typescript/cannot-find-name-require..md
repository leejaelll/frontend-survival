# Cannot find name 'require'.

```
Cannot find name 'require'. Do you need to install type definitions for node? 
Try `npm i --save-dev @types/node` and then add 'node' to the types field in your tsconfig.

const { PrismaClient } = require("@prisma/client");
```

👉🏻 tsconfig.json 에러

1. `npm i --save -dev @types/node`&#x20;
2. compilerOptions "types"에 "node" 추가
