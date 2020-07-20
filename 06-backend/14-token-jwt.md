```sh
sudo yarn add jsonwebtoken
sudo yarn add @types/jsonwebtoken -D
```

## src/services/AuthenticateUserService.ts

```ts
import { getRepository } from "typeorm";
import { compare } from "bcryptjs";

import { sign } from "jsonwebtoken";

import User from "../models/User";

interface Request {
  email: string;
  password: string;
}
interface Response {
  user: User;
  token: string;
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

    const token = sign({}, "9d91e3280732e82b6e04e22ec5b21a8f", {
      subject: user.id,
      expiresIn: "1d"
    });

    return {
      user,
      token
    };
  }
}

export default AuthenticateUserService;
```

## src/routes/sessions.routes.ts

```ts
import { Router } from "express";

import AuthenticateUserService from "../services/AuthenticateUserService";

const sessionsRouter = Router();

sessionsRouter.post("/", async (request, response) => {
  try {
    const { email, password } = request.body;

    const authenticateUser = new AuthenticateUserService();

    const { user, token } = await authenticateUser.execute({ email, password });

    delete user.password;

    return response.json({ user, token });
  } catch (error) {
    return response.status(400).json({ error: error.message });
  }
});

export default sessionsRouter;
```
