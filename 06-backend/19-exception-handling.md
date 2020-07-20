## src/errors/AppError.ts

```ts
class AppError {
  public message: string;
  public statusCode: number;

  constructor(message: string, statusCode = 400) {
    this.message = message;
    this.statusCode = statusCode;
  }
}

export default AppError;
```

## src/services/AuthenticateUserService.ts

```ts
import { getRepository } from "typeorm";
import { compare } from "bcryptjs";

import { sign } from "jsonwebtoken";
import authConfig from "../config/auth";

import AppError from "../errors/AppError";

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
      throw new AppError("Incorrect email/password combination.", 401);
    }

    const passwordMatched = await compare(password, user.password);
    if (!passwordMatched) {
      throw new AppError("Incorrect email/password combination.", 401);
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

## src/services/CreateAppointmentService.ts

```ts
import Appointment from "../models/Appointment";
import { startOfHour } from "date-fns";
import { getCustomRepository } from "typeorm";

import AppError from "../errors/AppError";

import AppointmentsRepository from "../repositories/AppointmentsRepository";

interface Request {
  provider_id: string;
  date: Date;
}

class CreateAppointmentService {
  public async execute({ date, provider_id }: Request): Promise<Appointment> {
    const appointmentsRepository = getCustomRepository(AppointmentsRepository);

    const appointmentDate = startOfHour(date);

    const findAppointmentInSameDate = await appointmentsRepository.findByDate(
      appointmentDate
    );

    if (findAppointmentInSameDate) {
      throw new AppError("This appointment is already booked");
    }

    const appointment = appointmentsRepository.create({
      provider_id,
      date: appointmentDate
    });

    await appointmentsRepository.save(appointment);

    return appointment;
  }
}

export default CreateAppointmentService;
```

## src/services/CreateUserService.ts

```ts
import { getRepository } from "typeorm";
import { hash } from "bcryptjs";

import AppError from "../errors/AppError";
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
      throw new AppError("Email address already used.");
    }

    const hashedPassword = await hash(password, 8);

    const user = usersRepository.create({
      name,
      email,
      password: hashedPassword
    });

    await usersRepository.save(user);

    return user;
  }
}

export default CreateUserService;
```

## src/services/UpdateUserAvatarService.ts

```ts
import { getRepository } from "typeorm";
import path from "path";
import fs from "fs";
import uploadConfig from "../config/upload";

import AppError from "../errors/AppError";

import User from "../models/User";

interface Request {
  user_id: string;
  avatarFilename: string;
}

class UpdateUserAvatarService {
  public async execute({ user_id, avatarFilename }: Request): Promise<User> {
    const usersRepository = getRepository(User);

    // verify if user exists
    const user = await usersRepository.findOne(user_id);
    if (!user) {
      throw new AppError("Only authenticated users can change avatar.", 401);
    }

    // verify if user had avatar. Delete before avatars
    if (user.avatar) {
      const userAvatarFilePath = path.join(uploadConfig.directory, user.avatar);
      const userAvatarFileExists = await fs.promises.stat(userAvatarFilePath);
      if (userAvatarFileExists) {
        await fs.promises.unlink(userAvatarFilePath);
      }
    }

    user.avatar = avatarFilename;

    await usersRepository.save(user);

    return user;
  }
}

export default UpdateUserAvatarService;
```

## src/middlewares/ensureAuthenticated.ts

```ts
import { Request, Response, NextFunction } from "express";
import { verify } from "jsonwebtoken";

import AppError from "../errors/AppError";

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
    throw new AppError("JWT token is missing", 401);
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
    throw new AppError("Invalid JWT Token", 401);
  }
}
```
