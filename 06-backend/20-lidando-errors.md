```sh
sudo yarn add express-async-errors
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
  const { provider_id, date } = request.body;

  const parsedDate = parseISO(date);

  const createAppointment = new CreateAppointmentService();

  const appointment = await createAppointment.execute({
    date: parsedDate,
    provider_id
  });

  return response.json(appointment);
});

export default appointmentsRouter;
```

## src/routes/sessions.routes.ts

```ts
import { Router } from "express";

import AuthenticateUserService from "../services/AuthenticateUserService";

const sessionsRouter = Router();

sessionsRouter.post("/", async (request, response) => {
  const { email, password } = request.body;

  const authenticateUser = new AuthenticateUserService();

  const { user, token } = await authenticateUser.execute({ email, password });

  delete user.password;

  return response.json({ user, token });
});

export default sessionsRouter;
```

## src/routes/users.routes.ts

```ts
import { Router } from "express";
import multer from "multer";
import uploadConfig from "../config/upload";

import CreateUserService from "../services/CreateUserService";
import UpdateUserAvatarService from "../services/UpdateUserAvatarService";

import ensureAuthenticated from "../middlewares/ensureAuthenticated";

const usersRouter = Router();
const upload = multer(uploadConfig);

usersRouter.post("/", async (request, response) => {
  const { name, email, password } = request.body;

  const createUser = new CreateUserService();

  const user = await createUser.execute({ name, email, password });

  delete user.password;

  return response.json(user);
});

// patch - atualiza uma informação só
usersRouter.patch(
  "/avatar",
  ensureAuthenticated,
  upload.single("avatar"),
  async (request, response) => {
    const updateUserAvatarService = new UpdateUserAvatarService();

    const user = await updateUserAvatarService.execute({
      user_id: request.user.id,
      avatarFilename: request.file.filename
    });

    delete user.password;

    return response.json({ user });
  }
);

export default usersRouter;
```

## src/server.ts

```ts
import "reflect-metadata";

import express, { Request, Response, NextFunction } from "express";
import "express-async-errors";
import routes from "./routes";
import uploadConfig from "./config/upload";
import AppError from "./errors/AppError";

import "./database";

const app = express();

app.use(express.json());
app.use("/files", express.static(uploadConfig.directory));
app.use(routes);

// deve ser dps das rotas
app.use(
  (err: Error, request: Request, response: Response, next: NextFunction) => {
    if (err instanceof AppError) {
      return response.status(err.statusCode).json({
        status: "error",
        message: err.message
      });
    }
    console.error(err);
    return response.status(500).json({
      status: "error",
      message: "Internal server error"
    });
  }
);

app.listen(3333, () => {
  console.log("Server started on port 3333!");
});
```
