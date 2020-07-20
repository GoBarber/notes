## Conteúdos

- Criando o contexto de autenticação
- Importar o contexto no app com um valor (solução temporária)
- Testando em SignIn

## src/hooks/AuthContext.ts

```ts
import { createContext } from "react";

interface AuthContextData {
  name: string;
}

const AuthContext = createContext<AuthContextData>({} as AuthContextData);

export default AuthContext;
```

## src/App.tsx

```tsx
import React from "react";
import GlobalStyle from "./styles/global";

import SignIn from "./pages/SignIn";
// import SignUp from './pages/SignUp';

import AuthContext from "./hooks/AuthContext";

const App: React.FC = () => {
  return (
    <>
      <AuthContext.Provider value={{ name: "Glauber" }}>
        <SignIn />
      </AuthContext.Provider>
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

import AuthContext from "../../hooks/AuthContext";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, Background } from "./styles";

const SignIn: React.FC = () => {
  const formRef = useRef<FormHandles>(null);

  const auth = useContext(AuthContext);
  console.log(auth);

  const handleSubmit = useCallback(async (data: object) => {
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
    } catch (err) {
      const errors = getValidationErros(err);
      formRef.current?.setErrors(errors);
    }
  }, []);

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
