## src/models/Appointment.ts

```ts
import { uuid } from "uuidv4";

class Appointment {
  id: string;

  provider: string;

  date: Date;

  constructor(provider: string, date: Date) {
    this.id = uuid();
    this.provider = provider;
    this.date = date;
  }
}

export default Appointment;
```

## src/routes/appointments.routes.ts

```ts
import { Router } from "express";

import { startOfHour, parseISO, isEqual } from "date-fns";
import Appointment from "../models/Appointment";

const appointmentsRouter = Router();

const appointments: Appointment[] = [];

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
      .json({ message: "This appointment is already booked" });
  }

  const appointment = new Appointment(provider, parsedDate);

  appointments.push(appointment);

  return response.json(appointment);
});

export default appointmentsRouter;
```
