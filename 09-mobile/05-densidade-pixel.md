## Conteúdos

- Editando SignIn
- Criando arquivos de tipos para aceitar imagens

## src/pages/SignIn/index.tsx

```tsx
import React from "react";
import { Image } from "react-native";

import { Container } from "./styles";

import logoImg from "../../assets/logo.png";

const SignIn: React.FC = () => {
  return (
    <Container>
      <Image source={logoImg} />
    </Container>
  );
};

export default SignIn;
```

## src/@types/index.d.ts

```ts
declare module "*.png";
```
