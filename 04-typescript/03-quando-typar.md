## Conteúdos

- A maioria dos tipos são auto detectados pelo visual code
- Caso haja importação em outro arquivo é necessário tipar
- **_ Toda vez que precisamos dadicionar tipagem, o editor irá avisar _**

## src/index.ts

```ts
import express from "express";
import { helloWorld } from "./routes";

const app = express();

app.get("/", helloWorld);

app.listen(3333);
```

## src/routes.ts

```ts
import express from "express";
import { helloWorld } from "./routes";

const app = express();

app.get("/", helloWorld);

app.listen(3333);
```
