## Conteúdos

- Criar pagina SignIn
  - Usar icons
- Estilizando SignIn
  - polished
- Importar SignIn em app.tsx

```sh
sudo yarn add react-icons
sudo yarn add polished
```

## src/pages/SignIn/index.tsx

```tsx
import React from "react";

import logoImg from "../../assets/logo.svg";

import { Container, Content, Background } from "./styles";
import { FiLogIn } from "react-icons/fi";

const SignIn: React.FC = () => {
  return (
    <Container>
      <Content>
        <img src={logoImg} alt='GoBarber' />

        <form action=''>
          <h1>Faça seu logon</h1>

          <input placeholder='E-mail' />
          <input type='password' placeholder='Senha' />

          <button type='submit'>Entrar</button>

          <a href='forgot'>Esqueci minha senha</a>
        </form>

        <a href='login'>
          <FiLogIn />
          Criar conta
        </a>
      </Content>
      <Background />
    </Container>
  );
};

export default SignIn;
```

## src/pages/SignIn/styles.ts

```css
import styled from 'styled-components';
import signInBackgroundImg from '../../assets/sign-in-background.png';
import { shade } from 'polished';

export const Container = styled.div`
  height: 100vh;

  display: flex;
  align-items: stretch;
`;

export const Content = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  width: 100%;
  max-width: 700px;

  form {
    margin: 80px 0;
    width: 340px;
    text-align: center;

    h1 {
      margin-bottom: 24px;
    }

    input {
      color: #f4ede8;
      background: #232129;
      border-radius: 10px;
      border: 2px solid #232129;
      padding: 16px;
      width: 100%;

      &::placeholder {
        color: #666360;
      }

      & + input {
        margin-top: 8px;
      }
    }

    button {
      background: #ff9000;
      height: 56px;
      border-radius: 10px;
      border: 0;
      padding: 0 16px;
      color: #312e38;
      width: 100%;
      font-weight: 500;
      margin-top: 16px;
      transition: background-color 0.2s;

      &:hover {
        background: ${shade(0.2, '#ff9000')};
      }
    }

    a {
      color: #f4ede8;
      display: block;
      margin-top: 24px;
      text-decoration: none;
      transition: color 0.2s;

      &:hover {
        color: ${shade(0.2, '#f4ede8')};
      }
    }
  }
  > a {
    color: #ff9000;
    display: block;
    margin-top: 24px;
    text-decoration: none;
    transition: color 0.2s;

    display: flex;
    align-items: center;

    svg {
      margin-right: 16px;
    }

    &:hover {
      color: ${shade(0.2, '#ff9000')};
    }
  }
`;

export const Background = styled.div`
  flex: 1;
  background: url(${signInBackgroundImg}) no-repeat center;
  background-size: cover;
`;

```

## src/App.tsx

```tsx
import React from "react";
import GlobalStyle from "./styles/global";

import SignIn from "./pages/SignIn";

const App: React.FC = () => {
  return (
    <>
      <SignIn />
      <GlobalStyle />
    </>
  );
};
export default App;
```
