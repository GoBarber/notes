## Conteúdos

- Instalar o react-router-dom
- Criar pasta routes e configurando ela
- Importar routes no app
- Redirecionar no SignIn e no SignOut
- Animação entre rotas no SignIn e SignIn
  - Colocar um div para fazer a animação nela
  - Usar o keyframes no styles

```sh
sudo yarn add react-router-dom
sudo yarn add @types/react-router-dom -D
```

## src/routes/index.tsx

```tsx
import React from "react";
import { Switch, Route } from "react-router-dom";

import SignIn from "../pages/SignIn";
import SignUp from "../pages/SignUp";

const Routes: React.FC = () => (
  <Switch>
    <Route path='/' exact component={SignIn} />
    <Route path='/signup' component={SignUp} />
  </Switch>
);

export default Routes;
```

## src/App.tsx

```tsx
import React from "react";
import { BrowserRouter as Router } from "react-router-dom";
import GlobalStyle from "./styles/global";

import Routes from "./routes";

import AppProvider from "./hooks";

const App: React.FC = () => {
  return (
    <Router>
      <AppProvider>
        <Routes />
      </AppProvider>
      <GlobalStyle />
    </Router>
  );
};
export default App;
```

## src/pages/SignIn/index.tsx

```tsx
import React, { useCallback, useRef } from "react";
import { FiLogIn, FiMail, FiLock } from "react-icons/fi";
import { FormHandles } from "@unform/core";
import { Form } from "@unform/web";
import * as Yup from "yup";
import { Link } from "react-router-dom";

import getValidationErros from "../../utils/getValidationErros";

import { useAuth } from "../../hooks/auth";
import { useToast } from "../../hooks/toast";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, AnimantionContainer, Background } from "./styles";

interface SignInFormData {
  email: string;
  password: string;
}

const SignIn: React.FC = () => {
  const formRef = useRef<FormHandles>(null);

  const { signIn } = useAuth();
  const { addToast } = useToast();

  const handleSubmit = useCallback(
    async (data: SignInFormData) => {
      formRef.current?.setErrors({});
      try {
        const schema = Yup.object().shape({
          email: Yup.string()
            .required("Email obrigatório")
            .email("Digite um e-mail válido"),
          password: Yup.string().required("Senha obrigatória"),
        });

        await schema.validate(data, {
          abortEarly: false,
        });

        await signIn({
          email: data.email,
          password: data.password,
        });
      } catch (err) {
        if (err instanceof Yup.ValidationError) {
          const errors = getValidationErros(err);
          formRef.current?.setErrors(errors);
        } else {
          addToast({
            type: "error",
            title: "Erro na autenticação",
            description:
              "Ocorreu um erro ao fazer login, cheque as credenciais",
          });
        }
      }
    },
    [signIn, addToast]
  );

  return (
    <Container>
      <Content>
        <AnimantionContainer>
          <img src={logoImg} alt='GoBarber' />

          <Form ref={formRef} onSubmit={handleSubmit}>
            <h1>Faça seu logon</h1>

            <Input name='email' icon={FiMail} placeholder='E-mail' />
            <Input
              name='password'
              icon={FiLock}
              type='password'
              placeholder='Senha'
            />

            <Button type='submit'>Entrar</Button>

            <a href='forgot'>Esqueci minha senha</a>
          </Form>

          <Link to='/signup'>
            <FiLogIn />
            Criar conta
          </Link>
        </AnimantionContainer>
      </Content>
      <Background />
    </Container>
  );
};

export default SignIn;
```

## src/pages/SignIn/styles.ts

```css
import styled, { keyframes } from 'styled-components';
import { shade } from 'polished';
import signInBackgroundImg from '../../assets/sign-in-background.png';

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

  img {
    @media only screen and (max-width: 600px) {
      padding-top: 100px;
    }
  }
`;

const appearFromLeft = keyframes`
  from {
    opacity: 0;
    transform: translateX(-50px)
  }
  to {
    opacity: 1;
    transform: translateX(0)
  }

`;

export const AnimantionContainer = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  animation: ${appearFromLeft} 1s;

  form {
    margin: 80px 0;
    width: 340px;
    text-align: center;

    h1 {
      margin-bottom: 24px;
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

    @media only screen and (max-width: 600px) {
      padding-bottom: 40px;
    }

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

## src/pages/SignUp/index.tsx

```tsx
import React, { useCallback, useRef } from "react";
import { FiArrowLeft, FiMail, FiUser, FiLock } from "react-icons/fi";
import { Form } from "@unform/web";
import { FormHandles } from "@unform/core";
import * as Yup from "yup";
import { Link } from "react-router-dom";

import getValidationErros from "../../utils/getValidationErros";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, AnimantionContainer, Background } from "./styles";

const SignUp: React.FC = () => {
  const formRef = useRef<FormHandles>(null);

  console.log(formRef);

  const handleSubmit = useCallback(async (data: object) => {
    formRef.current?.setErrors({});
    try {
      const schema = Yup.object().shape({
        name: Yup.string().required("Nome obrigatório"),
        email: Yup.string()
          .required("Email obrigatório")
          .email("Digite um e-mail válido"),
        password: Yup.string().min(6, "No mínimo menos 6 caracteres"),
      });

      await schema.validate(data, {
        abortEarly: false,
      });
    } catch (err) {
      const errors = getValidationErros(err);
      formRef.current?.setErrors(errors);
    }
  }, []);

  return (
    <Container>
      <Background />
      <Content>
        <AnimantionContainer>
          <img src={logoImg} alt='GoBarber' />

          <Form ref={formRef} onSubmit={handleSubmit}>
            <h1>Faça seu cadastro</h1>

            <Input name='name' icon={FiUser} placeholder='Nome' />
            <Input name='email' icon={FiMail} placeholder='E-mail' />
            <Input
              name='password'
              icon={FiLock}
              type='password'
              placeholder='Senha'
            />

            <Button type='submit'>Cadastrar</Button>
          </Form>

          <Link to='/'>
            <FiArrowLeft />
            Voltar para logon
          </Link>
        </AnimantionContainer>
      </Content>
    </Container>
  );
};

export default SignUp;
```

## src/pages/SignUp/styles.ts

```css
import styled, { keyframes } from 'styled-components';
import { shade } from 'polished';
import signUpBackgroundImg from '../../assets/sign-up-background.png';

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

  img {
    @media only screen and (max-width: 600px) {
      padding-top: 100px;
    }
  }
`;

const appearFromRight = keyframes`
  from {
    opacity: 0;
    transform: translateX(50px)
  }
  to {
    opacity: 1;
    transform: translateX(0)
  }

`;

export const AnimantionContainer = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  animation: ${appearFromRight} 1s;

  form {
    margin: 80px 0;
    width: 340px;
    text-align: center;

    h1 {
      margin-bottom: 24px;
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

    @media only screen and (max-width: 600px) {
      padding-bottom: 40px;
    }

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
  background: url(${signUpBackgroundImg}) no-repeat center;
  background-size: cover;
`;

```
