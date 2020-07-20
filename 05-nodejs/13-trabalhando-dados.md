## src/repositories/AppointmentsRepository.ts

```ts
import { isEqual } from "date-fns";
import Appointment from "../models/Appointment";

// DTO - Data Transfer Object
interface CreateAppointmentDTO {
  provider: string;
  date: Date;
}

class AppointmentsRepository {
  private appointments: Appointment[];

  constructor() {
    this.appointments = [];
  }

  public all(): Appointment[] {
    return this.appointments;
  }

  public findByDate(date: Date): Appointment | null {
    // Validation of same time
    const findAppointment = this.appointments.find(appointment =>
      isEqual(date, appointment.date)
    );

    return findAppointment || null;
  }

  public create({ provider, date }: CreateAppointmentDTO): Appointment {
    const appointment = new Appointment({ provider, date });

    this.appointments.push(appointment);

    return appointment;
  }
}

export default AppointmentsRepository;
```

## src/routes/appointments.routes.ts

```ts
import { Router } from "express";

import { startOfHour, parseISO } from "date-fns";

import AppointmentsRepository from "../repositories/AppointmentsRepository";

const appointmentsRouter = Router();
const appointmentsRepository = new AppointmentsRepository();

appointmentsRouter.get("/", (request, response) => {
  const appointments = appointmentsRepository.all();

  return response.json(appointments);
});

appointmentsRouter.post("/", (request, response) => {
  const { provider, date } = request.body;

  const parsedDate = startOfHour(parseISO(date));

  const findAppointmentInSameDate = appointmentsRepository.findByDate(
    parsedDate
  );

  if (findAppointmentInSameDate) {
    return response
      .status(400)
      .json({ message: "This appointment is already booked" });
  }

  const appointment = appointmentsRepository.create({
    provider,
    date: parsedDate
  });

  return response.json(appointment);
});

export default appointmentsRouter;
```

## src/models/Appointment.ts

```ts
import { uuid } from "uuidv4";

class Appointment {
  id: string;

  provider: string;

  date: Date;

  constructor({ provider, date }: Omit<Appointment, "id">) {
    this.id = uuid();
    this.provider = provider;
    this.date = date;
  }
}

export default Appointment;
```
