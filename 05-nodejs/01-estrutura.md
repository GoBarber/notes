```sh
sudo yarn init -y
sudo yarn add express
sudo yarn add typescript -D
sudo yarn add @types/express -D
sudo yarn tsc --init
sudo yarn add ts-node-dev -D
```

## tsconfig.json

```json
{
  ...
  "outDir": "./dist",
  "rootDir": "./src",
  ...
}
```

## src/server.ts

```ts
import express from "express";

const app = express();

app.get("/", (request, response) => {
  return response.json({ message: "hello world" });
});

app.listen(3333, () => {
  console.log("Server started on port 3333!");
});
```

## package.json

```json
{
  "name": "05-nodejs",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "tsc",
    "dev:server": "ts-node-dev --transpileOnly --ignore-watch node_modules src/server.ts"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "@types/express": "^4.17.6",
    "ts-node-dev": "^1.0.0-pre.44",
    "typescript": "^3.8.3"
  }
}
```
