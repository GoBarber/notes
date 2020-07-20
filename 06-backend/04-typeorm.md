```sh
  sudo yarn add typeorm pg
```

## ormconfig.json

```json
{
  "type": "postgres",
  "host": "localhost",
  "port": 5432,
  "username": "postgres",
  "password": "docker",
  "database": "gostack_gobarber"
}
```

## src/database/index.ts

```ts
import { createConnection } from "typeorm";

createConnection();
```

## src/server.ts

```ts
import express from "express";
import routes from "./routes";

import "./database";

const app = express();

app.use(express.json());

app.use(routes);
app.listen(3333, () => {
  console.log("Server started on port 3333!");
});
```

- Criar database usando postbird ou outro app

```sh
  sudo yarn typeorm
```
