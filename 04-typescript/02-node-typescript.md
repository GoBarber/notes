```sh
  sudo yarn init -y
```

```sh
  sudo yarn add typescript -D
  sudo yarn add express
  sudo yarn add @types/express -D
```

## src/index.ts

```ts
import express, { response } from "express";

const app = express();

app.get("/", (request, response) => {
  return response.json({ message: "Hello World" });
});

app.listen(3333);
```

# Iniciando o typescript no projeto

```sh
sudo yarn tsc --init
```

```sh
sudo yarn tsc
```

## tsconfig.json

```json
{
...
"outDir": "./dist"
...
}
```
