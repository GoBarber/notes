```sh
sudo yarn add multer
sudo yarn add @types/multer -D
sudo yarn typeorm migration:create -n AddAvatarFieldToUsers
```

## src/database/migrations/1588685302991-AddAvatarFieldToUsers.ts

```ts
import { MigrationInterface, QueryRunner, TableColumn } from "typeorm";

export default class AddAvatarFieldToUsers1588685302991
  implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.addColumn(
      "users",
      new TableColumn({
        name: "avatar",
        type: "varchar",
        isNullable: true
      })
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropColumn("users", "avatar");
  }
}
```

```sh
sudo yarn typeorm migration:run
```

## src/config/upload.ts

```ts
import path from "path";
import crypto from "crypto";
import multer from "multer";

const tmpFolder = path.resolve(__dirname, "..", "..", "tmp");

export default {
  directory: tmpFolder,
  storage: multer.diskStorage({
    destination: tmpFolder,
    filename(request, file, callback) {
      const filehash = crypto.randomBytes(10).toString("HEX");
      const filename = `${filehash}-${file.originalname}`;

      return callback(null, filename);
    }
  })
};
```

## src/routes/users.routes.ts

```ts
import { Router } from "express";
import multer from "multer";
import uploadConfig from "../config/upload";

import CreateUserService from "../services/CreateUserService";

import ensureAuthenticated from "../middlewares/ensureAuthenticated";

const usersRouter = Router();
const upload = multer(uploadConfig);

usersRouter.post("/", async (request, response) => {
  try {
    const { name, email, password } = request.body;

    const createUser = new CreateUserService();

    const user = await createUser.execute({ name, email, password });

    delete user.password;

    return response.json(user);
  } catch (error) {
    return response.status(400).json({ error: error.message });
  }
});

// patch - atualiza uma informação só
usersRouter.patch(
  "/avatar",
  ensureAuthenticated,
  upload.single("avatar"),
  async (request, response) => {
    return response.json({ ok: true });
  }
);

export default usersRouter;
```

## Criar pasta tmp fora do src

## Mantendo a pasta ignorada no gitignore

- Criar um arquivo .gitkeep dentro da pasta ignorada
- no .gitignore:

```
tmp/*
!tmp/.gitkeep
```
