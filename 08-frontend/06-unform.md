## Conteúdos

- Instalando o unform
- Aplicando o **Form**(@unform/web)no SignUp
- Usando o **useField**(@unform/core) no componente Input
- Usar o **useRef** para alterar o valor do campo

```sh
sudo yarn add @unform/core @unform/web
```

## src/pages/SignUp/index.tsx

```tsx
import React, { useCallback } from "react";
import { FiArrowLeft, FiMail, FiUser, FiLock } from "react-icons/fi";

import { Form } from "@unform/web";

import logoImg from "../../assets/logo.svg";

import Input from "../../components/Input";
import Button from "../../components/Button";

import { Container, Content, Background } from "./styles";

const SignUp: React.FC = () => {
  const handleSubmit = useCallback((data: object) => {
    console.log(data);
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

## src/components/Input/index.tsx

```tsx
import React, { InputHTMLAttributes, useEffect, useRef } from "react";
import { IconBaseProps } from "react-icons";

import { useField } from "@unform/core";

import { Container } from "./styles";

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  name: string;
  icon: React.ComponentType<IconBaseProps>;
}

const Input: React.FC<InputProps> = ({ name, icon: Icon, ...rest }) => {
  const inputRef = useRef(null);
  const { fieldName, defaultValue, error, registerField } = useField(name);

  useEffect(() => {
    registerField({
      name: fieldName,
      ref: inputRef.current,
      path: "value"
    });
  }, [registerField, fieldName]);

  return (
    <Container>
      {Icon && <Icon />}
      <input defaultValue={defaultValue} ref={inputRef} {...rest} />
    </Container>
  );
};

export default Input;
```
