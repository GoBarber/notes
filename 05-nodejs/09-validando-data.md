```
sudo yarn add date-fns
```

## src/routes/appointments.routes.ts

```ts
import { Router } from "express";
import { uuid } from "uuidv4";

import { startOfHour, parseISO, isEqual } from "date-fns";

const appointmentsRouter = Router();

interface Appoinment {
  id: string;
  provider: string;
  date: Date;
}

const appointments: Appoinment[] = [];

appointmentsRouter.post("/", (request, response) => {
  const { provider, date } = request.body;

  const parsedDate = startOfHour(parseISO(date));

  // Validation of same time
  const findAppointmentInSameDate = appointments.find(appointment =>
    isEqual(parsedDate, appointment.date)
  );
  if (findAppointmentInSameDate) {
    return response
      .status(400)
      .json({ message: "This appoinment is already booked" });
  }

  const appointment = {
    id: uuid(),
    provider,
    date: parsedDate
  };

  appointments.push(appointment);

  return response.json(appointment);
});

export default appointmentsRouter;
```
