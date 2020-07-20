## src/config/auth.ts

```ts
export default {
  jwt: {
    secret: "66e5e932824d40f1b8262eb1f181d207",
    expiresIn: "1d"
  }
};
```

## src/middlewares/ensureAuthenticated.ts

```ts
import { Request, Response, NextFunction } from "express";
import { verify } from "jsonwebtoken";

import authConfig from "../config/auth";

interface TokenPayload {
  iat: number;
  exp: number;
  sub: string;
}

export default function ensureAuthenticated(
  request: Request,
  response: Response,
  next: NextFunction
): void {
  // Validação do token JWT

  const authHeader = request.headers.authorization;
  if (!authHeader) {
    throw new Error("JWT token is missing");
  }

  const [, token] = authHeader.split(" ");
  try {
    const decoded = verify(token, authConfig.jwt.secret);

    const { sub } = decoded as TokenPayload;

    request.user = {
      id: sub
    };

    return next();
  } catch {
    throw new Error("Invalid JWT Token");
  }
}
```

## src/services/AuthenticateUserService.ts

```ts
import { getRepository } from "typeorm";
import { compare } from "bcryptjs";

import { sign } from "jsonwebtoken";
import authConfig from "../config/auth";

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

    const { secret, expiresIn } = authConfig.jwt;
    const token = sign({}, secret, {
      subject: user.id,
      expiresIn: expiresIn
    });

    return {
      user,
      token
    };
  }
}

export default AuthenticateUserService;
```

## src/routes/appointments.routes.ts

```ts
import { Router } from "express";
import { getCustomRepository } from "typeorm";
import { parseISO } from "date-fns";

import AppointmentsRepository from "../repositories/AppointmentsRepository";
import CreateAppointmentService from "../services/CreateAppointmentService";

import ensureAuthenticated from "../middlewares/ensureAuthenticated";

const appointmentsRouter = Router();

appointmentsRouter.use(ensureAuthenticated);

appointmentsRouter.get("/", async (request, response) => {
  const appointmentsRepository = getCustomRepository(AppointmentsRepository);
  const appointments = await appointmentsRepository.find();

  return response.json(appointments);
});

appointmentsRouter.post("/", async (request, response) => {
  try {
    const { provider_id, date } = request.body;

    const parsedDate = parseISO(date);

    const createAppointment = new CreateAppointmentService();

    const appointment = await createAppointment.execute({
      date: parsedDate,
      provider_id
    });

    return response.json(appointment);
  } catch (error) {
    return response.status(400).json({ error: error.message });
  }
});

export default appointmentsRouter;
```

## src/@types/express.d.ts

- mudar o tipo da biblioteca para forçar que o request do express tenha o tipo user

```ts
declare namespace Express {
  export interface Request {
    user: {
      id: string;
    };
  }
}
```
