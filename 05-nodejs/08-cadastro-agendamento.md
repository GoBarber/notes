```sh
sudo yarn add uuidv4
```

## src/server.ts

```ts
import express from "express";
import routes from "./routes";

const app = express();

app.use(express.json());

app.use(routes);
app.listen(3333, () => {
  console.log("Server started on port 3333!");
});
```

## src/routes/appointments.routes.ts

```ts
import { Router } from "express";
import { uuid } from "uuidv4";

const appointmentsRouter = Router();

const appointments = [];

appointmentsRouter.post("/", (request, response) => {
  const { provider, date } = request.body;
  const appointment = {
    id: uuid(),
    provider,
    date
  };

  appointments.push(appointment);

  return response.json(appointment);
});

export default appointmentsRouter;
```

## src/routes/index.ts

```ts
import { Router } from "express";
import appointmentsRouter from "./appointments.routes";

const routes = Router();

routes.use("/appointments", appointmentsRouter);

export default routes;
```
