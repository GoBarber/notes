## src/services/CreateUserService.ts

```ts
import { getRepository } from "typeorm";

import User from "../models/User";

interface Request {
  name: string;
  email: string;
  password: string;
}

class CreateUserService {
  public async execute({ name, email, password }: Request): Promise<User> {
    const usersRepository = getRepository(User);

    const checkUserExists = await usersRepository.findOne({ where: { email } });
    if (checkUserExists) {
      throw new Error("Email address already used.");
    }

    const user = usersRepository.create({ name, email, password });

    await usersRepository.save(user);

    return user;
  }
}

export default CreateUserService;
```

## src/routes/users.routes.ts

```ts
import { Router } from "express";

import CreateUserService from "../services/CreateUserService";

const usersRouter = Router();

usersRouter.post("/", async (request, response) => {
  try {
    const { name, email, password } = request.body;

    const createUser = new CreateUserService();

    const user = await createUser.execute({ name, email, password });

    delete user.password;

    return response.json(user);
  } catch (error) {
    return response.status(400).json({ error: error.message });
  }
});

export default usersRouter;
```

## src/routes/index.ts

```ts
import { Router } from "express";

import appointmentsRouter from "./appointments.routes";
import usersRouter from "./users.routes";

const routes = Router();

routes.use("/appointments", appointmentsRouter);
routes.use("/users", usersRouter);

export default routes;
```

- Substituir os providers enviado anteriormente nos appointments por provider_id
