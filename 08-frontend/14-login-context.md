## Conteúdos

- Criar a api com o axios
- Converter AuthContext para tsx.
- Criar o componente AuthProvider e exportar ele diretamente
- Criar o método de SignIn
- Usar a api no método SignIn
- Usar o context na página de SignIn para o login

```sh
sudo yarn add axios
```

## src/services/api.ts

```ts
import axios from "axios";

const api = axios.create({
  baseURL: "http://177.89.36.87:3333",
});

export default api;
```

## src/hooks/AuthContext.tsx

```tsx
import React, { createContext, useCallback } from "react";

import api from "../services/api";

interface SignInCredentials {
  email: string;
  password: string;
}
interface AuthContextData {
  name: string;
  signIn(credentials: SignInCredentials): Promise<void>;
}

export const AuthContext = createContext<AuthContextData>(
  {} as AuthContextData
);

export const AuthProvider: React.FC = ({ children }) => {
  const signIn = useCallback(async ({ email, password }: SignInCredentials) => {
    const response = await api.post("sessions", { email, password });

    console.log(response.data);
  }, []);

  return (
    <AuthContext.Provider value={{ name: "Glauber", signIn }}>
      {children}
    </AuthContext.Provider>
  );
};
```

## src/App.tsx

```tsx
import React from "react";
import GlobalStyle from "./styles/global";

import SignIn from "./pages/SignIn";
// import SignUp from './pages/SignUp';

import { AuthProvider } from "./hooks/AuthContext";

const App: React.FC = () => {
  return (
    <>
      <AuthProvider>
        <SignIn />
      </AuthProvider>
      <GlobalStyle />
    </>
  );
};
export default App;
```

## src/pages/SignIn/index.tsx

```tsx
import React, { useCallback, useRef, useContext } from "react";
import { FiLogIn, FiMail, FiLock } from "react-icons/fi";

import { FormHandles } from "@unform/core";
import { Form } from "@unform/web";

import * as Yup from "yup";

import getValidationErros from "../../utils/getValidationErros";

import { AuthContext } from "../../hooks/AuthContext";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, Background } from "./styles";

interface SignInFormData {
  email: string;
  password: string;
}

const SignIn: React.FC = () => {
  const formRef = useRef<FormHandles>(null);

  const { signIn } = useContext(AuthContext);

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

        signIn({
          email: data.email,
          password: data.password,
        });
      } catch (err) {
        const errors = getValidationErros(err);
        formRef.current?.setErrors(errors);
      }
    },
    [signIn]
  );

  return (
    <Container>
      <Content>
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
