## ormconfig.json

```json
{
  "type": "postgres",
  "host": "localhost",
  "port": 5432,
  "username": "postgres",
  "password": "docker",
  "database": "gostack_gobarber",
  "entities": ["./src/models/*.ts"],
  "migrations": ["./src/database/migrations/*.ts"],
  "cli": {
    "migrationsDir": "./src/database/migrations"
  }
}
```

## package.json

```json
...
  "scripts": {
    "build": "tsc",
    "dev:server": "ts-node-dev --transpileOnly --ignore-watch node_modules src/server.ts",
    "typeorm": "ts-node-dev ./node_modules/typeorm/cli.js"
  },
...
```

```sh
sudo yarn typeorm
sudo yarn typeorm migration:create -n CreateAppointments
```

## src/database/migrations/1588359209386-CreateAppointments.ts

```ts
import { MigrationInterface, QueryRunner, Table } from "typeorm";

export class CreateAppointments1588359209386 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.createTable(
      new Table({
        name: "appointments",
        columns: [
          {
            name: "id",
            type: "uuid",
            isPrimary: true,
            generationStrategy: "uuid",
            default: "uuid_generate_v4()"
          },
          {
            name: "provider",
            type: "varchar",
            isNullable: false
          },
          {
            name: "date",
            type: "timestamp with time zone",
            isNullable: false
          },
          {
            name: "created_at",
            type: "timestamp",
            default: "now()"
          },
          {
            name: "updated_at",
            type: "timestamp",
            default: "now()"
          }
        ]
      })
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropTable("appointments");
  }
}
```

```sh
sudo yarn typeorm migration:run
```

# Regras para alterar migration

### opção 1:

- Criar uma nova migration com a alteração

### opção 2

- Alterar a migration que acabou de criar
- OBS: Você só pode alterar a migration se ela não for enviada para o git

```sh
sudo yarn typeorm migration:revert
sudo yarn typeorm migration:show
```
