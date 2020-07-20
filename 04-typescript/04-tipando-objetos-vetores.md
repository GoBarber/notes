## src/services/CreateUser.ts

```ts
interface TechObject {
  title: string;
  experience: number;
}
interface CreateUserData {
  name?: string;
  email: string;
  password: string;
  techs: Array<string | TechObject>;
}

export default function createUser({
  name,
  email,
  password,
  techs
}: CreateUserData) {
  const user = {
    name,
    email,
    password,
    techs
  };

  return user;
}
```

## src/routes.ts

```ts
import { Request, Response } from "express";
import createUser from "./services/CreateUser";

export function helloWorld(request: Request, response: Response) {
  const user = createUser({
    name: "Glauber",
    email: "glaubercarv@gmail.com",
    password: "123456",
    techs: ["NodeJS", "ReactJS", { title: "React-Native", experience: 100 }]
  });
  return response.json(user);
}
```
