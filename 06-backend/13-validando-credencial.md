## src/routes/sessions.routes.ts

```ts
import { Router } from "express";

import AuthenticateUserService from "../services/AuthenticateUserService";

const sessionsRouter = Router();

sessionsRouter.post("/", async (request, response) => {
  try {
    const { email, password } = request.body;

    const authenticateUser = new AuthenticateUserService();

    const { user } = await authenticateUser.execute({ email, password });

    delete user.password;

    return response.json({ user });
  } catch (error) {
    return response.status(400).json({ error: error.message });
  }
});

export default sessionsRouter;
```

## src/routes/index.ts

```ts
import { Router } from "express";

import appointmentsRouter from "./appointments.routes";
import usersRouter from "./users.routes";
import sessionsRouter from "./sessions.routes";

const routes = Router();

routes.use("/appointments", appointmentsRouter);
routes.use("/users", usersRouter);
routes.use("/sessions", sessionsRouter);

export default routes;
```

## src/services/AuthenticateUserService.ts

```ts
import { getRepository } from "typeorm";
import { compare } from "bcryptjs";

import User from "../models/User";

interface Request {
  email: string;
  password: string;
}
interface Response {
  user: User;
}

class AuthenticateUserService {
  public async execute({ email, password }: Request): Promise<Response> {
    const usersRepository = getRepository(User);

    const user = await usersRepository.findOne({ where: { email } });
    if (!user) {
      throw new Error("Incorrect email/password combination.");
    }

    const passwordMatched = await compare(password, user.password);
    if (!passwordMatched) {
      throw new Error("Incorrect email/password combination.");
    }

    return {
      user
    };
  }
}

export default AuthenticateUserService;
```
