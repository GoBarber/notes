## Conteúdos

- Instalando o yup para fazer a validação
- Usando o yup para validação de campos do signup
- Colocar o abortEarly para mostrar todos os erros possíveis (sem cancelar no primeiro)

```
  sudo yarn add yup
  sudo yarn add @types/yup -D
```

## src/pages/SignUp/index.tsx

```tsx
import React, { useCallback } from "react";
import { FiArrowLeft, FiMail, FiUser, FiLock } from "react-icons/fi";

import { Form } from "@unform/web";

import * as Yup from "yup";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, Background } from "./styles";

const SignUp: React.FC = () => {
  const handleSubmit = useCallback(async (data: object) => {
    try {
      const schema = Yup.object().shape({
        name: Yup.string().required("Nome obrigatório"),
        email: Yup.string()
          .required("Email obrigatório")
          .email("Digite um e-mail válido"),
        password: Yup.string().min(
          6,
          "Senha deve conter no mínimo menos 6 caracteres"
        )
      });
      await schema.validate(data, {
        abortEarly: false
      });
    } catch (err) {
      console.log(err);
    }
  }, []);

  return (
    <Container>
      <Background />
      <Content>
        <img src={logoImg} alt='GoBarber' />

        <Form onSubmit={handleSubmit}>
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

        <a href='voltar'>
          <FiArrowLeft />
          Voltar para logon
        </a>
      </Content>
    </Container>
  );
};

export default SignUp;
```
