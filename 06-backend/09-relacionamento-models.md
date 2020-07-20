- Esse passo é apenas para simular uma alteração a partir da criação de uma migration nova

```sh
sudo yarn typeorm migration:create -n AlterProviderFieldToProviderId
```

## src/database/migrations/1588423491520-AlterProviderFieldToProviderId.ts

```ts
import {
  MigrationInterface,
  QueryRunner,
  TableColumn,
  TableForeignKey
} from "typeorm";

export default class AlterProviderFieldToProviderId1588423491520
  implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropColumn("appointments", "provider");
    await queryRunner.addColumn(
      "appointments",
      new TableColumn({
        name: "provider_id",
        type: "uuid",
        isNullable: true
      })
    );

    await queryRunner.createForeignKey(
      "appointments",
      new TableForeignKey({
        name: "AppointmentProvider",
        columnNames: ["provider_id"],
        referencedColumnNames: ["id"],
        referencedTableName: "users",
        onDelete: "SET NULL",
        onUpdate: "CASCADE"
      })
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropForeignKey("appointments", "AppointmentProvider");

    await queryRunner.dropColumn("appointments", "provider_id");

    await queryRunner.addColumn(
      "appointments",
      new TableColumn({
        name: "provider",
        type: "varchar"
      })
    );
  }
}
```

## tipo de relacionamentos

- Um para Um (OneToOne)
- Um para Muitos (OneToMany)
- Muitos para Muitos (ManyToMany)

## src/models/Appointment.ts

```ts
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  CreateDateColumn,
  UpdateDateColumn,
  ManyToOne,
  JoinColumn
} from "typeorm";

import User from "./User";

@Entity("appointments")
class Appointment {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  provider_id: string;

  @ManyToOne(() => User)
  @JoinColumn({ name: "provider_id" })
  provider: User;

  @Column("timestamp with time zone")
  date: Date;

  @CreateDateColumn()
  created_at: Date;

  @UpdateDateColumn()
  updated_at: Date;
}

export default Appointment;
```

- Se você precisar listar todos os agendamentos de um usuário específico você precisa fazer o relacionamento de user em appointments
