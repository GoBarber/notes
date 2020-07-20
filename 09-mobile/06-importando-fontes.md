## Conteúdos

- Colocar fonts em assets/fonts
- Observar nome do PostScripts name IOS
- Configurar para receber fonts

## react-native.config.js

```js
module.exports = {
  project: {
    ios: {},
    android: {},
  },
  assets: ["./assets/fonts/"],
};
```

## src/pages/SignIn/index.tsx

```tsx
import React from "react";
import { Image } from "react-native";

import { Container, Title } from "./styles";

import logoImg from "../../assets/logo.png";

const SignIn: React.FC = () => {
  return (
    <Container>
      <Image source={logoImg} />
      <Title>Faça seu logon</Title>
    </Container>
  );
};

export default SignIn;
```

## src/pages/SignIn/styles.ts

```css
import styled from 'styled-components/native';

export const Container = styled.View`
  flex: 1;
  align-items: center;
  justify-content: center;
`;
export const Title = styled.Text`
  font-size: 24px;
  color: #f4ede8;
  font-family: 'RobotoSlab-Medium';
  margin: 64px 0 24px;
`;

```
