```sh
sudo yarn add reflect-metadata
```

## src/models/Appointment.ts

```ts
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  CreateDateColumn,
  UpdateDateColumn
} from "typeorm";

@Entity("appointments")
class Appointment {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  provider: string;

  @Column("timestamp with time zone")
  date: Date;

  @CreateDateColumn()
  created_at: Date;

  @UpdateDateColumn()
  updated_at: Date;
}

export default Appointment;
```

## tsconfig.json

```json
...
"strictPropertyInitialization": false,
...
"experimentalDecorators": true
"emitDecoratorMetadata": true
```

## src/server.ts

```ts
import express from "express";
import routes from "./routes";
import "reflect-metadata";

import "./database";

const app = express();

app.use(express.json());

app.use(routes);
app.listen(3333, () => {
  console.log("Server started on port 3333!");
});
```
